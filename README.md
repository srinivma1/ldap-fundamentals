# ldap-fundamentals

Base DN :- This is the root domain in the LDAP directory from which search needs to be done.

Bind DN :- This is the user who is authorized to do search on LDAP directory.

Bind Password: Authorized users password who can search LDAP directory.

ldap url: ldap://hostname:389


# Install

Install OpenLDAP from following URL if you are installing on Windows:
http://www.userbooster.de/en/support/feature-articles/openldap-for-windows-installation.aspx


# Configuration


This tutorial is intended for programmers to install an OpenLDAP server in their computers, to grasp the essence of LDAP, and how to actually connect to one. 


1. Install OpenLDAP for Windows from http://www.userbooster.de/en/download/openldap-for-windows.aspx and follow its installation instruction. Install it on "C:\App\OpenLDAP"

2. Accept all the default. Use the BDB (Berkley Database) as the Backend Engine.

3. Your LDAP Server is now running. To see the service just open your Windows Services and search for OpenLDAP Service. If you dont want the service to run automatically everytime the Windows restart, just change it to Manual from the Properties Dialog.

4. Next, install LDAPExplorerTool from http://ldaptool.sourceforge.net/. And try to connect to your LDAP Server using these settings :

 a. Server Name or IP : According to your Computer Name or IP
 
 b. LDAP Port : 389 ; check the use default checkbox
 
 c. LDAP SSL Port : 636 ; check the use default checkbox
 
 d. Version : 3 (LDAP ver. 3)
 
 e. User DN : cn=Manager,dc=maxcrc,dc=com ; Uncheck the anonymous login.
 
 f. Password : secret
 
 g. Base DN (Just click the Guess Value button)
 
For everything else, just accept the default value

Click the Test Connection button. And after saving it, just click Open.

It should open an empty LDAP directory. Next we will try to add an actual value to it.

Create a file in C:\App\OpenLDAP\ldifdata, name it step1.ldif. The contents are :

// DEFINE DIT ROOT/BASE/SUFFIX ####
// uses RFC 2377 format
// replace maxcrc and com as necessary below
// or for experimentation leave as is

// dcObject is an AUXILLIARY objectclass and MUST
// have a STRUCTURAL objectclass (organization in this case)
// this is an ENTRY sequence and is preceded by a BLANK line

dn: dc=maxcrc,dc=com

dc: maxcrc

description: My wonderful company as much text as you want to place

objectClass: dcObject

objectClass: organization

o: Maxcrc, Inc.

// FIRST Level hierarchy - people 
// uses mixed upper and lower case for objectclass
// this is an ENTRY sequence and is preceded by a BLANK line

dn: ou=people, dc=maxcrc,dc=com

ou: people

description: All people in organisation

objectclass: organizationalunit

// SECOND Level hierarchy
// ADD a single entry under FIRST (people) level
// this is an ENTRY sequence and is preceded by a BLANK line
// the ou: Human Resources is the department name

dn: cn=Robert Smith,ou=people,dc=maxcrc,dc=com

objectclass: inetOrgPerson

cn: Robert Smith

cn: Robert J Smith

cn: bob  smith

sn: smith

uid: rjsmith

userpassword: rJsmitH

carlicense: HISCAR 123

homephone: 555-111-2222

mail: r.smith@example.com

mail: rsmith@example.com

mail: bob.smith@example.com

description: swell guy

ou: Human Resources

Save the file. And open a command line and run these command 

cd C:\App\OpenLDAP\ClientTools

ldapmodify.exe -a -x -h localhost -p 389 -D "cn=manager,dc=maxcrc,dc=com" -f d:\App\OpenLDAP\ldifdata\step1.ldif -w secret 

From your LDAP Explorer Tool menu, select File -> Open last configuration, and you will find the LDAP Directory is no longer empty.

Next lets add one of our own data to the LDAP Directory. Create a file in C:\App\OpenLDAP\ldifdata, name it samz.ldif. The contents :



dn: cn=Panji Pratomo,ou=people,dc=maxcrc,dc=com

objectclass: inetOrgPerson

cn: Panji Pratomo

cn: P Pratomo

cn: Panji P

sn: panji

uid: ppratomo

userpassword: SomePassword

carlicense: HISCAR 123

homephone: 555-111-2222

mail: panji.pratomo555@gmail.com

mail: panji.pratomo555@mysamz.com

mail: panji_pratomo555@yahoo.com

description: football maniac

ou: SOA



dn: cn=Fahmi Satrio,ou=people,dc=maxcrc,dc=com

objectclass: inetOrgPerson

cn: Fahmi Satrio

cn: F Satrio

cn: Mi

sn: fahmi

uid: fsatrio

userpassword: SomePassword

carlicense: HISCAR 123

homephone: 555-111-2222

mail: f.satrio222@gmail.com

mail: f.satrio222@mysamz.com

mail: guest108222@fif.co.id

description: tukang ngulik ga jelas

ou: SOA

Save the file. And open a command line and run these command 
cd C:\App\OpenLDAP\ClientTools

ldapmodify.exe -a -x -h localhost -p 389 -D "cn=manager,dc=maxcrc,dc=com" -f d:\App\OpenLDAP\ldifdata\samz.ldif -w secret 


From your LDAP Explorer Tool menu, select File -> Open last configuration.

  
  
 # Search

Command

1. All entries on host ldap.acme.com using port 389, and return all attributes and values

  ex: ldapsearch -h ldap.acme.com "objectClass=*"

2. Same as above, but return only attribute names

   ex: ldapsearch -A -h ldap.acme.com" objectClass=*"

3. All entries on host ldap.acme.com using port 389, return all attributes, and de-reference any aliases found

     ex: ldapsearch -a always -h ldap.acme.com "objectClass=*"

4. All entries on host ldap.acme.com using port 389, and return attributes=mail, cn, sn, givenname

     ex: ldapsearch -h ldap.acme.com "objectClass=*" mail cn sn givenname

5. (cn=Mike*) under base "ou=West,o=Acme, c=US" on host ldap.acme.com using port 389, and return all attributes and values

      ex: ldapsearch -b "ou=West,o=Acme,c=US" -h ldap.acme.com "(cn=Mike*)"

6. One level on host ldap.acme.com using port 389, and return all attributes and values

      ex: ldapsearch -s onelevel -h ldap.acme.com "objectClass=*"

7. Same as above, but limit scope to base

       ex: ldapsearch -s base -h ldap.acme.com "objectClass=*"

8. All entries on host ldap.acme.com using port 389; return all attributes and values; do not exceed the time limit of five seconds

       ex: ldapsearch -l 5 -h ldap.acme.com "objectClass=*"

9. All entries on host ldap.acme.com using port 389; return all attributes and values; do not exceed the size limit of five

        ex: ldapsearch -z 5 -h ldap.acme.com "objectClass=*"

10. All entries on host ldap.acme.com using port 389, binding as user "cn=John Doe,o=Acme" with a password of "password", and return all attributes and values in LDIF format

        ex: ldapsearch -h ldap.acme.com -D "cn=john doe,o=acme" -w password -L "objectClass=*"

11. Search the host ldap.acme.com using port 389. All attributes that anonymous are allowed to see are returned for the entry "cn=John Doe,o=Acme"

         ex:ldapsearch -h ldap.acme.com" -s base -b "cn=john doe,o=acme" objectClass=*"

12. All entries on a different host, bluepages.ibm.com, which is configured to listen for LDAP requests on port 391

          ex: ldapsearch -h bluepages.ibm.com -p 391 "objectClass=*"

13. Search bluepages.ibm.com on port 391. Doing a subtree search (default) starting in the organization "o=ibm" for any object type of Person who also has an attribute that matches any one of the attributes found in the OR filter. There is a timeout value of 300 seconds and the maximum number of entries to return is set to 1000. And only the DN (default) and CN will be returned. (This is a common filter for Web applications).

           ex: ldapsearch -h bluepages.ibm.com -p 391 -b "o=ibm" -l 300 -z 1000 "(&(objectclass=Person)(|(cn=mary smith*)(givenname=mary smith*)(sn=mary smith*)(mail=mary smith*)))" cn

14. Search bluepages.ibm.com on port 391 starting at the base entry "cn=HR Group,ou=Asia,o=IBM" with a time limit of 300 seconds and asking for all the members of this entry. (Another common filter in Web applications to determine group membership).

            ex: ldapsearch -h bluepages.ibm.com -p 391 -b "cn=HR Group,ou=Asia,o=IBM" -s base -l 300 "(objectclass=*)" member
