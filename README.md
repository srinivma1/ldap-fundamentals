# ldap-fundamentals

Base DN :- This is the root domain in the LDAP directory from which search needs to be done.

Bind DN :- This is the user who is authorized to do search on LDAP directory.

Bind Password: Authorized users password who can search LDAP directory.

ldap url: ldap://<hostname>:389
  
  
  Search

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
