first thing to do is run burp crawl then use these commands
```
subfinder -d indrive.com,aws.indriverapp.com -proxy http://127.0.0.1:8080 -silent -all -recursive -o subfinder.txt

---------------------------------------------------------------------------------

findomain -f targets | tee findomain.txt

---------------------------------------------------------------------------------

assetfinder --subs-only indrive.com | tee assetfinder.txt

---------------------------------------------------------------------------------

```


https://freedium.cfd/https://infosecwriteups.com/recon-to-master-the-complete-bug-bounty-checklist-95b80ea55ff0

```
amass enum -d indrive.com,indriver.com,aws.indriverapp.com,indriverapp.com -active -config ~/.config/amass/config.yaml | cut -d']' -f 2 | awk '{print $1}' | sort -u > amass.txt
```
