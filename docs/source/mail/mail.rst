Mail
#######

MUA, MTA, MDA
==========================================================

`Message Submission for Mail: RFC6409 <https://tools.ietf.org/html/RFC6409>`_

    MUA --smtp--> MTA --smtp--> MTA -> MDA --pop/imap--> MUA

MX
==========================================================

域名foo.com的MX记录指定负责接收邮件的服务器

yyy@bar.com ->  xxx@foo.com 

SMTP
==========================================================

`SMTP: RFC5321 <https://tools.ietf.org/html/rfc5321>`_

SMTP是邮件投递协议


明文端口 25

smtp over ssl 端口 465

msa (在smtp auth之后accept mail) 587

POP
==========================================================

拉取邮件

`RFC1939: Post Office Protocol - Version 3 <https://tools.ietf.org/html/rfc1939>`_

IMAP
==========================================================

拉取邮件

`RFC3501: INTERNET MESSAGE ACCESS PROTOCOL <https://tools.ietf.org/html/rfc3501>`_

