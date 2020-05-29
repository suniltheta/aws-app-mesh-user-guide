# App Mesh observability troubleshooting<a name="troubleshoot-observability"></a>

This topic details common issues that you may experience with App Mesh observability\.

## Unable to see AWS X\-Ray traces for my applications<a name="ts-observability-x-ray-traces"></a>

**Symptoms**  
Your application in App Mesh is not displaying X\-Ray tracing information in the X\-Ray console or APIs\.

**Resolution**  
To use X\-Ray in App Mesh, you must correctly configure components to enable communication between your application, sidecar containers, and the X\-Ray service\. Take the following steps to confirm that X\-Ray has been set up correctly:
+ Make sure that the X\-Ray container that is deployed with your application exposes UDP port `2000` and runs as user `1337`\. For more information, see the [Amazon ECS X\-Ray example](https://github.com/aws/aws-app-mesh-examples/blob/master/walkthroughs/howto-ecs-basics/deploy/2-meshify.yaml#L374-L386) on GitHub\.
+ Make sure that the Envoy container has tracing enabled\. If you are using the [App Mesh Envoy image](envoy.md), you can enable X\-Ray by setting the `ENABLE_ENVOY_XRAY_TRACING` environment variable to a value of `1` and the `XRAY_DAEMON_PORT` environment variable to `2000`\.
+ If you’ve instrumented X\-Ray in your application code with one of the [language\-specific SDKs ](https://docs.aws.amazon.com/xray/index.html), then make sure that it is configured correctly by following the guides for your language\.
+ If all of the previous items are configured correctly, then review the X\-Ray container logs for errors and follow the guidance in [Troubleshooting AWS X\-Ray](https://docs.aws.amazon.com/xray/latest/devguide/xray-troubleshooting.html)\. A more detailed explanation of X\-Ray integration in App Mesh can be found in [Integrating X\-Ray with App Mesh](http://aws.amazon.com/blogs/compute/integrating-aws-x-ray-with-aws-app-mesh/)\.

If your issue is still not resolved, then consider opening a [GitHub issue](https://github.com/aws/aws-app-mesh-roadmap/issues/new?assignees=&labels=Bug&template=issue--bug-report.md&title=Bug%3A+describe+bug+here) or contact [AWS Support](http://aws.amazon.com/premiumsupport/)\.

## Unable to see Envoy metrics for my applications in Amazon CloudWatch metrics<a name="ts-observability-envoy-metrics"></a>

**Symptoms**  
Your application in App Mesh is not emitting metrics generated by the Envoy proxy to CloudWatch metrics\.

**Resolution**  
When you use CloudWatch metrics in App Mesh, you must correctly configure several components to enable communication between your Envoy proxy, CloudWatch agent sidecar, and the CloudWatch metrics service\. Take the following steps to confirm that CloudWatch metrics for Envoy proxy have been setup correctly:
+ Make sure that you are using the CloudWatch agent image for App Mesh\. For more information, see [App Mesh CloudWatch agent](https://github.com/aws-samples/aws-app-mesh-cloudwatch-agent) on GitHub\.
+ Make sure that you have configured the CloudWatch agent for App Mesh appropriately by following the platform\-specific usage instructions\. For more information, see [App Mesh CloudWatch agent](https://github.com/aws-samples/aws-app-mesh-cloudwatch-agent#usage) on GitHub\.
+ If all of the previous items are configured correctly, then review the CloudWatch agent container logs for errors and follow the guidance provided in [Troubleshooting the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/troubleshooting-CloudWatch-Agent.html)\.

If your issue is still not resolved, then consider opening a [GitHub issue](https://github.com/aws/aws-app-mesh-roadmap/issues/new?assignees=&labels=Bug&template=issue--bug-report.md&title=Bug%3A+describe+bug+here) or contact [AWS Support](http://aws.amazon.com/premiumsupport/)\.

## Unable to configure custom sampling rules for AWS X\-Ray traces<a name="ts-observability-custom-sampling"></a>

**Symptoms**  
Your application is using X\-Ray tracing, but you are unable to configure sampling rules for your traces\.

**Resolution**  
This is a known issue\. For more information, see the [Support Dynamic X\-Ray sampling configuration](https://github.com/aws/aws-app-mesh-roadmap/issues/95) issue on GitHub\. The Envoy proxy allows configuration of sampling rules through static configuration\. For more information, see [AWS X\-Ray Tracer configuration](https://www.envoyproxy.io/docs/envoy/latest/api-v3/config/trace/v3/xray.proto.html?highlight=tracing) in the Envoy documentation\. The Envoy proxy cannot currently be configured to dynamically configure sampling rules through the App Mesh service APIs\. 

If your issue is still not resolved, then consider opening a [GitHub issue](https://github.com/aws/aws-app-mesh-roadmap/issues/new?assignees=&labels=Bug&template=issue--bug-report.md&title=Bug%3A+describe+bug+here) or contact [AWS Support](http://aws.amazon.com/premiumsupport/)\.