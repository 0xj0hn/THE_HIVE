# ⭕ LLMNR Poisoning

## LLMNR

**link local multi-casting name resolution** is used to identify hosts when DNS fails to do so .previously known as NBT-NS (netbios). key flaw is that the srvices utilize a users username and NTLMv2 hash when apropriately responded to.

![](<../../../.gitbook/assets/image (204).png>)

we use the responder to sniff the request while inside the network this is the first tool to run even before nmap or nessus because we want to listen to as much traffic as we can and scanners themselves are going to generate a lot of traffic and will cause a lot of responses back to our machine.

```
responder -I vboxnet0 -rdwv
```

#### the trigger for this attack can be any wrong query in the domain for example when a user from a workstation is trying to access a share and a typo happens the DNS service will fail to locate the resource. this is when responder comes in and claims to know the resource location and asks the client for the NTLM hash. another example is when the client is pointed to the attacker machine:

![](<../../../.gitbook/assets/image (211).png>)

![](<../../../.gitbook/assets/image (208).png>)

then we crack the hash with hashcat :

```
hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt --force
```

## other forms of LLMNR and NBT-NS attacks

{% embed url="https://www.4armed.com/blog/llmnr-nbtns-poisoning-using-responder/" %}
