# NSX Plugin Operator

Warning: This application is currently available as a Technical Preview only. Please refer to https://docs.eda.dev/25.8/connect/vmware-nsx/ for more information.

## Summary

This application provides the means to deploy instances of the plugin that integrates VMware NSX with the Cloud Connect application.  
  
After installing this app, a new CRD, NSXPluginInstance, is exposed. Creating such a CR will deploy an instance of the NSX plugin.

## Dependencies
This plugin depends on the VMware Plugin Operator App being installed.
