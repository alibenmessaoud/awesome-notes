## CORS

Cross-origin resource sharing, or CORS, is security feature of IE10+, Chrome 4+, Firefox 3.5+ or almost every version of browser released after 2012 except Opera Mini.

When CORS are configured on server that is available on domain `website.com` then resources from that domain those are requested trough AJAX must be initiated from assets that is served from that same domain.

CORS are disabled by default which means that there is no adequate server handler that will configure CORS then any AJAX request trying to access your resources will be rejected because web browsers are respectful to the `CORS policy`.

## XSS

XSS stands for Cross Site Scripting and it is injection type of attack. It is listed as 7th out of top 10 vulnerabilities identified by OWASP in 2017. Cross site scripting is the method where the attacker injects malicious script into trusted website.

The attacker comes on your website and finds unprotected input field and enters malicious script instead of expected value. After that, whenever that value should be displayed to other users it will execute malicious code. Malicious script can be the involved in DDoS attack or similar.

Do string escape on front end, as well!

## CSRF

Cross site request forgery or CSRF is a type of attack that occurs when a malicious web site, email, etc. causes a user's web browser to perform an unwanted action on an other trusted site where the user is authenticated. This vulnerability is possible when browser automatically sends authorization resource, such as session cookie, IP address or similar with each request.

Attacker tricks user to execute script (by clicking on SPAM link in email or similar) that will send request to your API based on your data session; 

One of the most common pattern is usage of `CSRF token`.

There is no protection on CSRF attack if your web application is XSS vulnerable!