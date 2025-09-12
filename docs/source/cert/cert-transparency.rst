Certificate Transparency
==========================


        struct {
          Version sct_version;
          LogID id;                  // hash(CT log server's public key)
          uint64 timestamp;          
          CtExtensions extensions;   
          digitally-signed {         
            ...
          };
        } SignedCertificateTimestamp;



        signature = Sign(LogPrivateKey, hash( 
            SCTVersion || SignatureType || Timestamp || IssuerKeyHash || TBSCertificate || Extensions
        ))


