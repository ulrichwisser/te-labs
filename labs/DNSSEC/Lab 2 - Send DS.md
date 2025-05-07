
<img src="https://github.com/yakanho/training/assets/54844453/321060e5-fc84-40f7-8caa-846d0a68494b" alt="ICANN" style="zoom:25%;" />

------

# Lab Generate your domain Delegation Signer (DS) and send it to your parent

```
Created by: Yazid AKANHO
Modified by: -
Current version: 2024020400
Previous version:-
```

------

As a reminder, your domain is grpX.<lab_domain>.te-labs.training, and your parent is <lab_domain>.te-labs.training.

> [!IMPORTANT]
>
> Your parent is expecting your DS file name in the following format: **DS_YOUR-KSK-key-tag.grpX**. Reminder to replace 'X' with your group number. For example, if your KSK tag is 2347 and your group number if 12, your DS file name should be DS_2347.grp12.



Start by creating a directory to store your DS files for your domain and make sure BIND has "write" access :

```
# mkdir -p /var/lib/bind/zones/ds
# chown -R bind:bind /var/lib/bind/zones/ds
```



### Generate your DS record

Execute the following command to get the DS record and save it in the required file:

```
# dnssec-dsfromkey /var/lib/bind/keys/KgrpX.<lab_domain>.te-labs.training.+XYZ+YOUR-KSK-key-tag.key > /var/lib/bind/zones/ds/DS_YOUR-KSK-key-tag.grpX
```

or you could extract the DS directly from the DNSKEY by querying your domain.

```
# dig @localhost dnskey grpX.<lab_domain>.te-labs.training | dnssec-dsfromkey -f - grpX.<lab_domain>.te-labs.training > /var/lib/bind/zones/ds/DS_YOUR-KSK-key-tag.grpX
```

Verify the content of the generated file:

```
# cat /var/lib/bind/zones/ds/DS_YOUR-KSK-key-tag.grpX
```

Which should contain something similar to the following line:

```
grpX.<lab_domain>.te-labs.training. IN DS YOUR-KSK-key-tag 8 2 018A86C0139BA5500AC87A5BAD8FB5D8D4F9672C319B34DB5A7F3BC10A424D6E
```



### Push the DS to your parent

On the web page for your group
