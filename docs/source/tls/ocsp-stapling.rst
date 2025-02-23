ocsp Stapling
##########################################################

`The case for “OCSP Must-Staple” <https://www.grc.com/revocation/ocsp-must-staple.htm>`_

`OCSP Stapling in Firefox <https://blog.mozilla.org/security/2013/07/29/ocsp-stapling-in-firefox/>`_


Multiple Cert Status Request
==========================================================

RFC6961 

TLS Multiple Certificate Status Request extension

支持服务器在TLS握手时发送多个ocsp响应

请求的数据结构

.. raw::

    struct {
       CertificateStatusType status_type;
       uint16 request_length; 
       select (status_type) {
         case ocsp: OCSPStatusRequest;
         case ocsp_multi: OCSPStatusRequest;
       } request;
     } CertificateStatusRequestItemV2;

     enum { ocsp(1), ocsp_multi(2), (255) } CertificateStatusType;

     struct {
       ResponderID responder_id_list<0..2^16-1>;
       Extensions request_extensions;
     } OCSPStatusRequest;

     opaque ResponderID<1..2^16-1>;
     opaque Extensions<0..2^16-1>;

     struct {
       CertificateStatusRequestItemV2
                        certificate_status_req_list<1..2^16-1>;
     } CertificateStatusRequestListV2;

应答的数据结构

.. raw::

    struct {
       CertificateStatusType status_type;
       select (status_type) {
         case ocsp: OCSPResponse;
         case ocsp_multi: OCSPResponseList;
       } response;
     } CertificateStatus;

     opaque OCSPResponse<0..2^24-1>;

     struct {
       OCSPResponse ocsp_response_list<1..2^24-1>;
     } OCSPResponseList;
