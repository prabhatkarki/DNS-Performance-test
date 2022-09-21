# DNS-Performance-test
This provides details on DNS performance test on BIGIP/SPK in Openshift cluster. We use dnsperf tool for this purpose
1. Deploy the dns-perf pod using the deployment yaml in the k8s namespace that egresses via BIGIP/SPK.


2. exec into the dns-perf pod
oc get pods -n spk-app-01
NAME                         READY   STATUS    RESTARTS   AGE
dnsperf-659597b55b-7r7xp     1/1     Running   103        104d

oc exec -it dnsperf-659597b55b-7r7xp  -n spk-app-01 -- bash
root@dnsperf-659597b55b-7r7xp:~# 

3. Creat a queryfile and add DNS A fqdns to be queried
touch queryfile
echo “stablcurcolbtest.nat64.ws.vici.verizon.com.   A” >queryfile
echo “betadev31.nat64.ws.vici.verizon.com.   A”>>queryfile

Run the dnsperf -s <DNS Server IP> in coming DNS queries will be served by forwarding VS in BIGIP/SPK and will forward to destination IP of DNS server.

dnsperf -d queryfile -s 96.239.250.46 -l 10 -c 100 -Q 60


dnsperf -help
DNS Performance Testing Tool
Nominum Version 2.1.0.0

Usage: dnsperf [-f family] [-s server_addr] [-p port] [-a local_addr]
               [-x local_port] [-d datafile] [-c clients] [-T threads]
               [-n maxruns] [-l timelimit] [-b buffer_size] [-t timeout]
               [-e] [-D] [-y [alg:]name:secret] [-q num_queries]
               [-Q max_qps] [-S stats_interval] [-u] [-v] [-h]
  -f address family of DNS transport, inet or inet6 (default: any)
  -s the server to query (default: 127.0.0.1)
  -p the port on which to query the server (default: 53)
  -a the local address from which to send queries
  -x the local port from which to send queries (default: 0)
  -d the input data file (default: stdin)
  -c the number of clients to act as
  -T the number of threads to run
  -n run through input at most N times
  -l run for at most this many seconds
  -b socket send/receive buffer size in kilobytes
  -t the timeout for query completion in seconds (default: 5)
  -e enable EDNS 0
  -D set the DNSSEC OK bit (implies EDNS)
  -y the TSIG algorithm, name and secret
  -q the maximum number of queries outstanding (default: 100)
  -Q limit the number of queries per second
  -S print qps statistics every N seconds
  -u send dynamic updates instead of queries
  -v verbose: report each query to stdout
  -h print this help
