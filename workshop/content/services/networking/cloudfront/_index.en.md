***

## title: “Amazon CloudFront”&#xA;menutitle: “Amazon CloudFront”&#xA;weight:2&#xA;chapter: true&#xA;pre:”<b></b>“&#xA;AWS Services: Amazon CloudFront

Amazon CloudFront is a web service that accelerates the distribution of static and dynamic web content, such as .html, .css, .js, and image files, to users. CloudFront distributes content through a global network of data centers called edge locations. When requesting content you're serving with CloudFront, the user is routed to the edge location with the lowest latency (time delay) so that the content is delivered with the best possible performance.

*   If the content is already at the edge location with the lowest latency, CloudFront will deliver it immediately.

*   If the content is not at this edge location, CloudFront retrieves it from a defined origin, such as an Amazon S3 bucket, a MediaPackage channel, or an HTTP server (for example, a web server), that you identified as the source for the definitive version of the content.

***

**References**

*   [Amazon CloudFront](https://docs.aws.amazon.com/pt_br/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)
