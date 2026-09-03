DMARC Results

--- Connection parameters ---
Source IP address: 0.0.0.0
Hostname: example1.com
Sender: user@example2.com

--- SPF ---
Domain: example2.com
Identity: RFC5321.MailFrom
Auth Result: PASS
DMARC Alignment: PASS

--- DKIM ---
Domain: example2.com
Selector: MBO0001
Algorithm:  (2048-bit)
Auth Result: PASS
DMARC Alignment: PASS

--- DMARC ---
RFC5322.From domain: example2.com
Policy (p=): none
SPF: PASS
DKIM: PASS
DMARC Result: PASS

--- Final verdict ---
DMARC does not take any specific action regarding message delivery. Generally, this means that the message will be successfully delivered. However, it's important to note that other factors like spam filters can still reject or quarantine a message.

