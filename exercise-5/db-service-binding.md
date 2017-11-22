bx target --cf
bx service create compose-for-mysql Standard "[name]"
The format of the name must match the regex [a-z0-9]([-a-z0-9]*[a-z0-9])?(\.[a-z0-9]([-a-z0-9]*[a-z0-9])?)* (e.g. 'example.com').
bx service key-create "[name]" Credentials-1

bx cs clusters
bx cs cluster-service-bind andylab default "[name]"
kubectl get secrets

bx service key-show "[name]" "Credentials-1" |grep "mysql:"
"uri": "mysql://admin:CAHQDXDYGAWISLZM@sl-us-south-1-portal.15.dblayer.com:28498/compose"
