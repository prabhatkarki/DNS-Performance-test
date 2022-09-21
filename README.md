# DNS-Performance-test
This provides details on DNS performance test on BIGIP/SPK in Openshift cluster. We use dnsperf tool for this purpose


touch queryfile
echo “stablcurcolbtest.nat64.ws.vici.verizon.com.   A” >queryfile
echo “betadev31.nat64.ws.vici.verizon.com.   A”>>queryfile

dnsperf -d queryfile -s 96.239.250.46 -l 10 -c 100 -Q 60

