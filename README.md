# Porkbun & Mailbox.org Custom Domain setup
## Overview
This project shows my journey setting up my own custom email domain for personal use. This will also include the lessons learnt, and instructions/guides on any further iterations I make (including DMARC, DKIM setup in the future.)

## Key Concepts I learnt about

The usage of SPF (Sender Policy Framework), DKIM (DomainKeys Identified Mail) and DMARC (Domain-based Message Authentication, Reporting and Conformance) can be simplified significantly

> Rules -> Validation -> Resolution

- The SPF determines what mail servers are authorised to send email on behalf of your domain `example.com`. In our case, we exclusively allow `mailbox.org` and its associated IPs to send emails on our behalf, preventing our emails from being spoofed by malicious actors.
- The DKIM acts like a hash, ensuring that an email sent from your domain has not been altered in transit (integrity) and further ensures that the email really came from you (authenticity)
- DMARC defines what happens should an email fail the SPF/DKIM checks. The owner can make emails that fail checks be rejected, quarantine (sent into spam folder) or be sent normally.

## Why did I choose Porkbun to buy my domain?
I chose Porkbun primarily because of their WHOIS Privacy feature (which will prevent your details from being looked up on the WHOIS registry), their user-friendly dashboard and their competitive pricing - as I was able to get my domains for a small price compared to other registrars like GoDaddy.

## Why did I choose Mailbox.org?
This will be a long section, as there are many factors I had to balance when choosing my personal email host - I will create a separate document you can see to view my in depth analysis, but the shortened version is:

Mailbox.org has a solid amount of features for 3 Euros/Month, as well as support for 50 Custom Email Domains (which can be very helpful in the future) and their extensive knowledge base which has been excellent in supporting my journey. You can find their knowledge base [here](https://kb.mailbox.org/en/private/faq/)

## Setup
Prerequisites:
- Already purchased/own your domain
- A `standard` tier Mailbox.org subscription (or free trial if you want to see whether this works for you)
- An email client (application) that is officially supported by Mailbox.org (or your personal email provider)
    - I personally recommend Thunderbird for both Windows/Android, but you can view the list of supported applications [here](https://kb.mailbox.org/en/private/faq/compatible-web-browsers/)
- Internet Access

This guide will be specific to Porkbun specifically, instructions might differ slightly for other registrars.

### 1) Add your custom domain to your Mailbox.org account

Make your way through **Settings > All Settings > Email Addresses > Add External alias** (See screenshots below)

Add your custom email address as an external alias. For example `example123@domain.com`

Then Press **save**

![A screenshot showing the settings icon to press](/images/topbar-settings.png)

![Screenshot showing the all settings option](/images/all-settings.png)

![A screenshot showing the email addresses setting](/images/email-addresses-settings.png)

### 2) Add the Mailbox Security Key to your Domain's DNS settings

Now that you have added your custom alias - you should get this error. But don't worry! We will fix this now

![A screenshot showing an error validating external domain](/images/mailbox-security-code.png)

Take a note of the given code. As we are on Porkbun, we will use the **__first__** and **last** part of the code.
__`p30oe995fc9105322345b6277ac3b1i2eb45106j5j7`__.example.com. IN TXT `5ahgui6nsdj5kkk666l333m7m7k7n5k6nnnn5bb4`

**NOTE: This is used to verify that you own the domain you want to use as your email address - and you will only need to do this once. After this, any new email addresses on the same domain can be added with no issue, by following Step 1.**

Now, head to your Porkbun domain dashboard and make your way to the **DNS & Nameservers** area:

![An image showing the DNS & Nameservers area](/images/dns-general-settings.png)

*You want to also enable Porkbun DNSSEC, this helps prevent cache poisoning - where an unsuspecting user is sent to what looks like your domain but is instead an attacker controlled domain. As domains are saved in cache, this means that every time victims go to (what they think is) your domain, they instead go to the attackers website.*

Now, create a new TXT Record and fill the *Host* with the **__first__** part of the code, and the *Answer/Value* with the **last** part of the code.

![An image showing the TXT record option](/images/txt-dropdown.png)

![An image showing the host/answer setting](/images/host-reponse-setting.png)

Now, go back to your Mailbox.org dashboard and press `save` again on your custom email domain.

### 3) Set up forwarding to your mailbox
Go back to your Porkbun Domain DNS settings and add the following domains, these just route mail to your inbox:
| Domain | Record Type | Priority | Target server |
| ------------- | ------------- | ------------- | ------------- |
| `[YOUR_DOMAIN]`  | `MX` | `10` | `mxext1.mailbox.org.` |
| `[YOUR_DOMAIN]`  | `MX` | `10` | `mxext2.mailbox.org.` |
| `[YOUR_DOMAIN]`  | `MX` | `10` | `mxext3.mailbox.org.` |
| `[YOUR_DOMAIN]`  | `MX` | `10` | `mxext4.mailbox.org.` |

### 4) Add Additional Security to your Email Domain
Finally, we should add some security to our email domain. There is a risk malicious actors may try to impersonate us by spoofing our email address, as we have not limited who can send emails from our domain - so we will add a set of rules that must be followed.

Below is a table of Domains you need to add to your Domain's DNS on Porkbun:

| Hostname | Record Type | Target |
| ------------- | ------------- | ------------- |
| `@`  | `TXT`  | `v=spf1 include:mailbox.org ~all`  | 
| `_dmarc.[YOUR_DOMAIN]`  | `TXT` | `v=DMARC1;p=none;rua=mailto:postmaster@[YOUR_DOMAIN]` |
| `MBO0001._domainkey.[YOUR_DOMAIN]`  | `CNAME` | `MBO0001._domainkey.mailbox.org.` |
| `MBO0002._domainkey.[YOUR_DOMAIN]`  | `CNAME` | `MBO0002._domainkey.mailbox.org.` | 
| `MBO0003._domainkey.[YOUR_DOMAIN]`  | `CNAME` | `MBO0003._domainkey.mailbox.org.` |
| `MBO0004._domainkey.[YOUR_DOMAIN]`  | `CNAME` | `MBO0004._domainkey.mailbox.org.` |

The DMARC Target, `p=none` could be set to multiple different variables:
- `p=none` Allows emails that fail verification to be sent normally, but reports are sent back to you.
- `p=quarantine` Automatically sends emails that fail verification to the user's spam folder.
- `p=reject` All emails that fail verifications are blocks and not sent

As mentioned above, these act as the rules, validation and resolutions for your domain.
Then, add `postmaster@[YOUR_DOMAIN]` to your Mailbox and you are finished!

Once that is complete, your custom email domain is all set and secured, although this may take a few minutes to fully register! Feel free to repeat Step 1 to create aliases for your email.

If you want to further check the security of your domain, use [Learn DMARC](https://www.learndmarc.com/) as I found this useful in understanding how DMARC works.

### Bonus Step: Filtering Aliases using folders
Now our various email aliases are working, we have the option on Mailbox.org to filter email sent to our aliases to a folder of our choice, rather than it being sent to our unified inbox - allowing for greater organisation.

First, we want to create a new folder. __*Do not add email account*__

![An image showing what icon to press to create a new folder](/images/folder-screenshot.png)

Now make your way through **Settings > All Settings > Read & Write Email > Rules**

![An image showing the Rules Setting within Read & Write Email](/images/rules-setting-screenshot.png)

Create a new rule by pressing the "Add new Rule button"

![An image showing the button to press to add new rule](/images/add-new-rule.png)

And finally, we want to define the `condition` and `action`. 
Create a condition `to`, and input your email address for your custom domain. Then, create a `file to` action, and set it to the folder of your choice(See below)

![An image showing the condition and action parameters](/images/rule-param.png)

Now press `save and apply rule now` and you're done! Try send a test email to your new email address, and it should filter into your folder - this may take a minute or two.
Once you have received your test email - congrats! You have set up your own custom email domain using Porkbun and Mailbox.org!

### Lessons Learnt
This project taught me how to prevent spoofing using email authentication methods like DKIM/DMARC, as well as how much the older internet was built around trust - and not security. This highlights to me the importance of checking these records when investigating suspicious emails in the future, and why SOC analysts check them.

I further gained more knowledge about web security (Cache Poisoning specifically), learning what happens when an attacker tries this type of attack, and how using settings like DNSSEC prevents it.

### Results
You can view the DMARC results [here](/dmarc-results.md) or view the screenshot below:
![An image showing the DMARC results](/images/dmarc.png)