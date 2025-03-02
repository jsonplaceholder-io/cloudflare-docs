---
pcx_content_type: troubleshooting
source: https://support.cloudflare.com/hc/en-us/articles/360020991331-Adding-vendor-specific-DNS-records-to-Cloudflare
title: Adding vendor-specific DNS records to Cloudflare
---

# Adding vendor-specific DNS records to Cloudflare



## Adding DNS records for vendors

This article requires prior knowledge of DNS record management via the Cloudflare dashboard.  To learn more, refer to Cloudflare's article on [managing DNS records](https://developers.cloudflare.com/dns/manage-dns-records/how-to/create-dns-records).

**Google**

Add the following MX records:

| **Name** | **TTL** | **Record Type** | **Priority**  | **Target** |
| --- | --- | --- | --- | --- |
| @ | Auto | MX | 1 | ASPMX.L.GOOGLE.COM |
| @ | Auto | MX | 5 | ALT1.ASPMX.L.GOOGLE.COM |
| @ | Auto | MX | 5 | ALT2.ASPMX.L.GOOGLE.COM |
| @ | Auto | MX | 10 | ALT3.ASPMX.L.GOOGLE.COM |
| @ | Auto | MX | 10 | ALT4.ASPMX.L.GOOGLE.COM |

Once added, the DNS records appear similar to the following in Cloudflare's **DNS** app:

[Test the Google Apps email configuration](https://toolbox.googleapps.com/apps/checkmx/check).

Add a _CNAME record_ for Google App Engine to Cloudflare DNS.

For example, if the domain is _www.example.com_, the _CNAME record_ is similar to:


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">www  CNAME  ghs.googlehosted.com</span></div></span></span></span></code></pre>{{</raw>}}

To configure a redirect for a Google Apps domain, refer to [Google’s guide on URL forwarding](https://support.google.com/a/answer/53340?hl=en).

**Amazon**

AWS customers must update their domain's nameservers to point to the Cloudflare nameservers listed in the **Overview** app of the Cloudflare dashboard:

1.  Log into AWS.
2.  Click **My Account** in the top-right of the navigation bar.
3.  Select **AWS Management Console** from the dropdown.
4.  Click **Services** and select **Route 53**.
5.  Update nameservers in two places:

-   Click **Hosted zones** and select the domain to update with Cloudflare's nameservers.
-   Edit the nameservers to point to Cloudflare's nameservers.

-   Click **Registered domains**.
-   Select the domain to update with Cloudflare's nameservers.
-   Click **Add or edit name servers**.

Consult Amazon’s documentation on how to [c](https://docs.aws.amazon.com/quickstarts/latest/s3backup/step-1-create-bucket.html)[reate an Amazon S3 bucket](https://docs.aws.amazon.com/quickstarts/latest/s3backup/step-1-create-bucket.html).

Note the full host URL assigned to the bucket.

Add a _CNAME record_ for the AWS bucket in Cloudflare DNS. For example, if the full host URL of the bucket is _files.example.com_, add a _CNAME record_ similar to the following:


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">files  CNAME  files.example.com.s3.amazonaws.com</span></div></span></span></span></code></pre>{{</raw>}}

Refer to Amazon’s documentation about [SES and verification settings](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/easy-dkim.html).

Find the _TXT_ and _CNAME_ verification records that Amazon provides.

Add the records to Cloudflare DNS.  For example, if the Cloudflare domain is _example.com_, the DNS records are similar to the following:


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">example.com  TXT  &quot;fmxqxT/icOYx4aA/bEUrDPMeax9/s3frblS+niixmqk=&quot;</span></div></span><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">verificationstring._domainkey.example.com  CNAME  verificationstring.dkim.amazonses.com</span></div></span></span></span></code></pre>{{</raw>}}

Refer to [Amazon's ELB help content](http://docs.amazonwebservices.com/ElasticLoadBalancing/latest/DeveloperGuide/using-domain-names-with-elb.html) for guidance on ELB configuration at Amazon.

1.  Add a _CNAME record_ to Cloudflare for the hostname; for example: _elb_
2.  In the Cloudflare **DNS** app, replace **Domain name** with the ELB target: _<AWS hostname>.<region>._elb.amazonaws.com is the proper _CNAME_ target format (for example: _my-cool-cachepp-1344276401.eu-west-1._elb.amazonaws.com).
3.  Reach out to AWS support to determine _AWS hostname_ or _region_.

**Microsoft**

Follow Microsoft’s instructions on [configuring Azure DNS settings](https://www.windowsazure.com/en-us/develop/net/common-tasks/custom-dns-web-site/).

Add Azure’s required records to Cloudflare DNS.

For example, if the domain is _example.com_, the record format is similar to:


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">example.com  A  203.0.113.1</span></div></span><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">www.example.com  CNAME  example.azurewebsites.net</span></div></span></span></span></code></pre>{{</raw>}}

For verification records, refer to Azure’s documentation on [creating domain verification records](https://docs.microsoft.com/en-us/office365/admin/dns/create-dns-records-for-azure-dns-zones?view=o365-worldwide#add-a-txt-record-for-verification).

**Miscellaneous vendors**

You can configure Cloudflare to work with ClickFunnels.  The process requires updating your Cloudflare DNS settings.

The following articles from ClickFunnels outline how to best configure the two services for your site:

-   [Adding a Cloudflare subdomain](https://help.clickfunnels.com/hc/en-us/articles/360005906774-Adding-A-Cloudflare-Subdomain-)
-   [Cloudflare CNAME record](https://help.clickfunnels.com/hc/en-us/articles/360005906094-Cloudflare-CNAME-Record)

Refer to the examples below for adding proper Zoho DNS records to Cloudflare. In all examples, replace _example.com_ with the actual domain name:

-   Add Zoho _MX records_:


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">example.com  MX  mx.zohomail.com (set Priority to 10)</span></div></span><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">example.com  MX  mx2.zohomail.com (set Priority to 20)</span></div></span></span></span></code></pre>{{</raw>}}

-   (Optional) Add an _SPF record_:


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">example.com  TXT  v=spf1 mx include:zoho.com ~all</span></div></span></span></span></code></pre>{{</raw>}}

-   (Optional) To access mail through a [custom Zoho URL](https://adminconsole.wiki.zoho.com/domains/CustomURL.html), add a _CNAME record_:


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">mail  CNAME  business.zoho.com</span></div></span></span></span></code></pre>{{</raw>}}

-   (Optional) To add a [Zoho domain validation record](https://www.zoho.com/mail/help/adminconsole/domain-verification.html):


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">zb******** CNAME  business.zoho.com</span></div></span></span></span></code></pre>{{</raw>}}

Typically, the DNS records are similar to the list below. Replace _example.com_ with the actual domain name:


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">email  CNAME  sendgrid.net</span></div></span><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">example.com  SPF  v=spf1 a mx include:sendgrid.net ~all</span></div></span><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">example.com  TXT  v=spf1 a mx include:sendgrid.net ~all</span></div></span><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">mtpapi._domainkey.EXAMPLE.com  CNAME  dkim.sendgrid.net.</span></div></span><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">smtpapi._domainkey.e.EXAMPLE.COM  CNAME  dkim.sendgrid.net</span></div></span></span></span></code></pre>{{</raw>}}

Refer to Ning's documentation on [Custom Domains and DNS entries](http://www.ning.com/help/?p%3D2870).

If the Ning custom domain is _www.example.com_, add a _CNAME_ and an _A record_ as follows:


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">www.example.com  CNAME  example.ning.com.</span></div></span><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">example.ning.com  A  208.82.16.68</span></div></span></span></span></code></pre>{{</raw>}}

After Ning verifies the domain, change the grey-cloud icon to an orange-cloud for the Ning DNS records so traffic can proxy to Cloudflare.

Consult SmugMug documentation for the latest details on DNS record requirements. Typically, add _CNAME records_ for SmugMug similar to the following:


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">photo  CNAME  domains.smugmug.com</span></div></span><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">photos  CNAME  domains.smugmug.com</span></div></span></span></span></code></pre>{{</raw>}}

After SmugMug verifies the domain, change the grey-cloud icon to an orange-cloud for the SmugMug DNS records so traffic can proxy to Cloudflare.

Refer to [Mandrill's article on DNS records](http://help.mandrill.com/entries/22030056-How-do-I-add-DNS-records-for-my-sending-domains-) for the latest details on DNS record requirements.

Mandrill requires addition of _SPF_ and _DKIM records_. Obtain the DNS record values from Mandrill.

Add the _SPF_ and _DKIM records_ as _TXT records_ in the Cloudflare DNS app.

For example, if _example.com_ is the Mandrill domain, add DNS records similar to the following.


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">example.com  TXT  v=spf1 include:spf.mandrillapp.com ?all</span></div></span><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">mandrill._domainkey.example.com  TXT  v=DKIM1\; (values from Mandrill)</span></div></span></span></span></code></pre>{{</raw>}}

Configure Rackspace CloudFiles via _CNAME record_. Consult the [Rackspace documentation](https://docs.rackspace.com/support/how-to/using-cnames-with-cloud-files-containers/).

Confirm the correct _CNAME_ target with Rackspace support.

An example _CNAME record_ appears as follows:


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">rack  CNAME  e0978.r18.cf2.rackcdn.com</span></div></span></span></span></code></pre>{{</raw>}}

After ensuring that your domain nameservers are set to Cloudflare, 

1.  Log in to the Cloudflare dashboard.
2.  Click the appropriate Cloudflare account for the domain where you will add records.
3.  Ensure the proper domain is selected.
4.  Click the **DNS** app.
5.  Ensure that all four (4) A records and the www CNAME from Squarespace are mark _Proxied_ (as shown below).
6.  Ensure that the 'verify.squarespace.com' CNAME record is marked _DNS Only_ (as shown below).

![screenshot of the cloudflarecontent.com DNS records from Squarespace
](/support/static/dns_ui_update_squarespace_records.png)

If set up properly, your Squarespace DNS Settings page will now indicate that your 'Settings contain problems.' **This is the expected behavior**. 

![Old URL: https://support.cloudflare.com/hc/article_attachments/360039675171/squarespace_dns_settings.png
Article IDs: 360020991331 | Adding vendor-specific DNS records to Cloudflare
](/support/static/hc-import-squarespace_dns_settings.png)

Now that your traffic is being sent through Cloudflare, Squarespace and your site's visitors will see Cloudflare IP addresses. This causes Squarespace console to assume your site is misconfigured as Cloudflare IPs are returned instead of Squarespace assigned addresses. As long as you've configured Cloudflare DNS appropriately (above steps 1-6), your Squarespace site should now be working through Cloudflare.

If _example.com_ is the custom domain, add DNS records to Cloudflare similar to these below:


{{<raw>}}<pre class="CodeBlock CodeBlock-with-rows CodeBlock-scrolls-horizontally CodeBlock-is-light-in-light-theme CodeBlock--language-txt" language="txt"><code><span class="CodeBlock--rows"><span class="CodeBlock--rows-content"><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">example.com  A  66.6.44.4</span></div></span><span class="CodeBlock--row"><span class="CodeBlock--row-indicator"></span><div class="CodeBlock--row-content"><span class="CodeBlock--token-plain">www.example.com  CNAME  domains.tumblr.com</span></div></span></span></span></code></pre>{{</raw>}}

___

## Related resources

-   [Managing Cloudflare DNS records](https://developers.cloudflare.com/dns/manage-dns-records/how-to/create-dns-records)
-   [CNAME Flattening](https://developers.cloudflare.com/dns/additional-options/cname-flattening)
