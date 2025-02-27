[link-wallarm-mode-override]:       ../../admin-en/configure-parameters-en.md#wallarm_mode_allow_override

[img-mode-rule]:        ../../images/user-guides/rules/wallarm-mode-rule-with-safe-blocking.png

# Filtration mode rule

The filtration mode allows you to enable and disable the blocking of requests to various parts of a web application.

To set a filtration mode, create a *Set traffic filtration mode* rule and select the appropriate mode.

The filtration mode can take one of the following values:

* **Default**: the system will work in accordance with the parameters specified in the NGINX configuration files.
* **Disable**: the analysis and filtration of requests are disabled completely.
* **Monitorig**: the requests are analyzed and displayed in the interface but they are not blocked even if they are originated from [blacklisted](../ip-lists/blacklist.md) IPs.
* **Safe blocking**: malicious requests are blocked only if they are originated from [greylisted IPs](../ip-lists/greylist.md).
* **Blocking**: malicious requests are blocked and displayed in the interface.

To implement this rule, the NGINX configuration files must permit [centralized management of the operation mode][link-wallarm-mode-override].

## Creating and applying the rule

--8<-- "../include/waf/features/rules/rule-creation-options.md"

## Default instance of rule

Wallarm automatically creates the instance of the `Set traffic filtration mode` rule on the [default](../../user-guides/rules/view.md#default-rules) level. The system sets its value on the basis of [general filtration mode](../../admin-en/configure-wallarm-mode.md#setting-up-the-general-filtration-rule-in-wallarm-console) setting.

This instance of the rule cannot be deleted. To change its value, modify [general filtration mode](../../admin-en/configure-wallarm-mode.md#setting-up-the-general-filtration-rule-in-wallarm-console) setting of the system.

As all the other default rules, the `Set traffic filtration mode` default rule is [inherited](../../user-guides/rules/view.md) by all branches.

## Example: Disabling Request Blocking During User Registration

**If** the following conditions take place:

* new user registration is available at *example.com/signup*
* it is better to overlook an attack than to lose a customer

**Then**, to create a rule disabling blocking during user registration

1. Go to the *Rules* tab
1. Find the branch for `example.com/signup`, and click *Add rule*
1. Choose *Set traffic filtration mode*
1. Choose operation mode *monitoring*
1. Click *Create*

![!Setting traffic filtration mode][img-mode-rule]
