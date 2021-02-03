# Upgrading Istio
https://istio.io/latest/docs/setup/upgrade/
*Upgrading across more than one minor version (e.g., 1.6.x to 1.8.x) in one step is not officially tested or recommended.*

## In-place Upgrades

The istioctl upgrade command performs an upgrade of Istio. Before performing the upgrade, it checks that the Istio installation meets the upgrade eligibility criteria. Also, it alerts the user if it detects any changes in the profile default values between Istio versions.

The upgrade command can also perform a downgrade of Istio.

**DON'T USE IT ON PRODUCTION**


## Canary Upgrades
**1. Upgrading Control plane**

To install a new revision called canary, you would set the revision field as follows:

In a production environment, a better revision name would correspond to the Istio version. However, you must replace . characters in the revision name, for example, revision=1-6-8 for Istio 1.6.8, because . is not a valid revision name character.
```
$ istioctl install --set revision=canary
```

**2. Upgrading Data plane**

Unlike istiod, Istio gateways do not run revision-specific instances, but are instead in-place upgraded to use the new control plane revision

To upgrade the namespace test-ns, remove the istio-injection label, and add the istio.io/rev label to point to the canary revision. The istio-injection label must be removed because it takes precedence over the istio.io/rev label for backward compatibility.
```
$ kubectl label namespace test-ns istio-injection- istio.io/rev=canary
```
After the namespace updates, you need to restart the pods to trigger re-injection. One way to do this is using:
```
$ kubectl rollout restart deployment -n test-ns
```

**3. Uninstall old control plane**

After upgrading both the control plane and data plane, you can uninstall the old control plane. For example, the following command uninstalls a control plane of revision 1-6-5:
```
$ istioctl x uninstall --revision 1-6-5
```
If the old control plane does not have a revision label, uninstall it using its original installation options, for example:
```
$ istioctl x uninstall -f manifests/profiles/default.yaml
```
