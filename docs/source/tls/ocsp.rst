OCSP (online certificate status protocol)
##########################################################

`Addressing the challenges of TLS, revocation, and OCSP <https://www.fastly.com/blog/addressing-challenges-tls-revocation-and-ocsp>`_

`No, don't enable revocation checking <https://www.imperialviolet.org/2014/04/19/revchecking.html>`_

`SSL certificate revocation and how it is broken in practice <https://medium.com/@alexeysamoshkin/how-ssl-certificate-revocation-is-broken-in-practice-af3b63b9cb3>`_

`OCSP Validation with OpenSSL <https://akshayranganath.github.io/OCSP-Validation-With-Openssl/>`_

`Specification documentfor OCSP <https://www.nets.eu/dk-da/kundeservice/nemid-tjenesteudbyder/Documents/TU-pakken/Tools/Specifikationsdokumenter/Specification%20document%20for%20OCSP%20EN.pdf>`_

`OpenSSL: Manually verify a certificate against an OCSP <https://raymii.org/s/articles/OpenSSL_Manually_Verify_a_certificate_against_an_OCSP.html>`_

`Using OpenSSL to run an OCSP query for an SSL Certificate <https://knowledge.digicert.com/solution/SO19587.html>`_


证书撤销的信息理论上可以用CRL批量获取、或OCSP在线逐个查询。

事实上多数浏览器采用OCSP，因为CRL文件太大没必要。

而由于ocsp存在时延、隐私问题，建议采用ocsp stapling，由服务器代理获取TLS验证所需的相关证书链信息。

google chrome的人反对ocsp主要基于时延的考虑，此外认为soft-fail模式的存在会使得攻击者阻断client->ocsp的通信，使得client接受revocated证书，对MITM攻击的防御并没有什么提升。

而避免CRL/OCSP的解决方案可以是浏览器厂商周期性更新CRL的一个子集，里面只包含"chrome/firefox认为"是"重点"的revocation信息，浏览器定时更新即可。例如chrome的CRLSet，firefox的OneCRL。优点明显，不用下载CRL的全集大文件，避免了OCSP的时延开销；缺点是人家说你重要你就重要，说你不重要你就不重要，此外就是凭什么相信某些"安全"浏览器的问题。

或者强制 `OCSP Must-Staple <https://scotthelme.co.uk/ocsp-must-staple/>`_
，实用性有限。

问题的根源在于证书的有效时间一般一签就是几年，中途撤销信息必须能及时推送。如果OCSP Stapling + ocsp Response定期更新（例如有效期设置为几天），对多数网站是一个相对可接受的折中。

ocsp request
==========================================================

`RFC6066, Section 8 Certificate Status Request <https://tools.ietf.org/html/rfc6066#page-14>`_

ocsp请求的数据结构，其中ResponderID是指client信任的CSP responders列表，如果列表为空则表示server已预先知道这些responders无需重复指定。

.. note::

         struct {
              CertificateStatusType status_type;
              select (status_type) {
                  case ocsp: OCSPStatusRequest;
              } request;
          } CertificateStatusRequest;

          enum { ocsp(1), (255) } CertificateStatusType;

          struct {
              ResponderID responder_id_list<0..2^16-1>;
              Extensions  request_extensions;
          } OCSPStatusRequest;

          opaque ResponderID<1..2^16-1>;
          opaque Extensions<0..2^16-1>;

ocsp应答的数据结构

.. note::

     struct {
          CertificateStatusType status_type;
          select (status_type) {
              case ocsp: OCSPResponse;
          } response;
      } CertificateStatus;

      opaque OCSPResponse<1..2^24-1>;

OCSP spec
==========================================================

`RFC6960 X.509 Internet Public Key Infrastructure Online Certificate Status Protocol - OCSP <https://tools.ietf.org/html/rfc6960>`_

签发OCSP应答的private key必须来源于以下之一：
- 签发request里查询的证书的CA
- responder的public key被requestor信任
- CA授权的responder，该responder的证书里标识了CA授权该证书签发OCSP应答签名

注意，签发OCSP应答的private key不需要与签发request里查询的证书的private key一致。签发request里查询证书的上层CA，只需签发一张OCSP专用证书，在extended key usage extension里标识该证书具备签发该系列OCSP应答的权限即可。

request里可以带签名，相当于client向ocsp responder表明自己以某个身份发起了此次证书状态查询。

responder证书可以带CRL References，标识它是从哪里获得证书revoke信息，扩展为id-pkix-ocsp-crl。

client如何查询OCSP证书是否还有效：
- CA配置client默认信任在有效期内的OCSP证书, id-pkix-ocsp-nocheck。该场景下，ocsp证书有效期较短，频繁renew。
- CA指定如何查询OCSP证书有效性，例如指定 CRL Distribution Points 或 Authority Information Access，详见RFC5280。
- 由client本地策略决定是否查询。

4.1.1 节是OCSP Request格式说明，4.2.1 节是OCSP Response格式说明

.. note::

    ResponseData ::= SEQUENCE {
      version              [0] EXPLICIT Version DEFAULT v1,
      responderID              ResponderID,
      producedAt               GeneralizedTime,
      responses                SEQUENCE OF SingleResponse,
      responseExtensions   [1] EXPLICIT Extensions OPTIONAL }

    SingleResponse ::= SEQUENCE {
          certID                       CertID,
          certStatus                   CertStatus,
          thisUpdate                   GeneralizedTime,
          nextUpdate         [0]       EXPLICIT GeneralizedTime OPTIONAL,
          singleExtensions   [1]       EXPLICIT Extensions OPTIONAL }


注意看2.4节的4个时间设置，The thisUpdate and nextUpdate fields define a recommended validity interval.

.. note::

     thisUpdate: responder知道该status的时间
     nextUpdate: 该status在哪个时间之前有效
     producedAt: ocsp responder签发该response的时间
     revocationTime: 该证书被撤销的时间

