# Porkbun & Mailbox.org Custom Domain setup
## Overview
This project shows my journey setting up my own custom email domain for business use. This will also include the lessons learnt, and instructions/guides on any further iterations I make (including DMARC, DKIM setup in the future.)

## Why did I choose Porkbun to buy my domain?
I chose Porkbun primarily because of their WHOIS Privacy feature (which will prevent your details from being looked up on the WHOIS registry), their user-friendly dashboard and their competitive pricing - as I was able to get my domains for a small price compared to other registrars like GoDaddy.

## Why did I choose Mailbox.org?
This will be a long section, as there are many factors I had to balance when choosing my personal email host - I will create a seperate document you can see to view my in depth analysis, but the shortened version is:

Mailbox.org has a solid amount of features for 3 Euros/Month, as well as support for 50 Custom Email Domains (which can be very helpful in the future) and their extensive knowledge base which has been excellent in supporting my journey. You can find their knowledge base [here](https://kb.mailbox.org/en/private/faq/)

## Setup
Prerequisites:
- Already purchased/own your domain
- A `standard` tier Mailbox.org subscription (or free trial if you want to see whether this works for you)
- An email client (application) that is officialy supported by Mailbox.org (or your personal email provider)
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

Take a note of the given code. As we are on Porkbun, we will use the first part of the code before our domain.
`p30oe995fc9105322345b6277ac3b1i2eb45106j5j7`.example.com.

**NOTE: This is used to verify that you own the domain you want to use as your email address - and you will only need to do this once. After this, any new email addresses on the same domain can be added with no issue, by following Step 1.**




