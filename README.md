# Overview and notes 

For this exercise I'll be running three VM's on an open network adapter.
Note that everything performed here is in a prebuilt controlled environment setup on my own CPU for demonstration purposes.

Let's get started.

# Reconnaissance

Also known as exploration, reconnaissance refers to gathering information about the network - or victim. For that, we'll first run an _nmap_ command in our linux cmd to explore who else is on in our network.
<img width="705" height="851" alt="Screenshot_1" src="https://github.com/user-attachments/assets/2fe0b30e-a51d-4ab1-8372-fedd599b5417" />

Note that another IP address (192.168.1.210 - Windows 10) was found on the same network as us.

A quick ping check to make sure it's replying.
<img width="1506" height="829" alt="Screenshot_2" src="https://github.com/user-attachments/assets/2dbc711c-007b-4e5e-bd59-da1a3f55dbb3" />

For a more detailed script of all ports we are going to run an _nmap -sC_
<img width="1667" height="857" alt="Screenshot_3" src="https://github.com/user-attachments/assets/2b844cba-093a-4443-9aea-5af973825edb" />


With all this open ports, including _SSH (22)_, _HTTP (80) and MSRPC/TCP (135)_ many vulnerabilities could be exployted, such as SQL injection on the open HTTP port, brute-force the SSH port and the known EternalBlue precursor on TCP.
For now, I'll demonstrate how to steal a credential by deploying a Metasploit2 VM to this environment and adding some complexity to the exercise.


# Intrusion 

Let's again run _nmap_ in our network to see who else might be up.
<img width="689" height="792" alt="Screenshot_4" src="https://github.com/user-attachments/assets/3a6701fb-759f-4927-92be-25997be37df4" />

See that we now have 3 hosts up. The first IP address - Metasploit2 - it's a machine prebuilt to expose all ports, as you can see most ports are open. Let's dig further.
Take your time to read in detail all ports and access that is allowed.

<img width="701" height="867" alt="Screenshot_5" src="https://github.com/user-attachments/assets/8c156497-5784-416a-a648-9048b6e2f110" />

<img width="693" height="866" alt="Screenshot_6" src="https://github.com/user-attachments/assets/0ffaecba-c422-44aa-998d-9f0118b84ba0" />

<img width="699" height="487" alt="Screenshot_7" src="https://github.com/user-attachments/assets/4f06e40f-d7f6-40d4-a251-f0493d2cfc90" />

On port _8180/TCP_ we've got an Apache Tomcat server running on a HTTP page.
<img width="697" height="520" alt="Screenshot_8" src="https://github.com/user-attachments/assets/66e0d1fb-6346-4d2c-9e7c-c79652f499e2" />

Let's see if we can access the server via our browser on Kali.
<img width="872" height="749" alt="Screenshot_9" src="https://github.com/user-attachments/assets/e1e8637a-237c-4ecc-9c07-78141ca940ce" />

<img width="959" height="891" alt="Screenshot_10" src="https://github.com/user-attachments/assets/6566b59b-dbe8-4323-956d-cd0e768a3acd" />

There we have it. This is what happens in a real life situation when servers and webservers aren't setup properly. Their data is easily accessible to any threat actor to exploit the system. 

# Credential stealing

Port _80_ was also open and hosting Metasploitable 2 webpage.
<img width="1041" height="649" alt="Screenshot_11" src="https://github.com/user-attachments/assets/992b722b-b3dd-4c01-968f-394d2082f069" />

Let's check if we can access their webpage and play around with it.
<img width="938" height="896" alt="Screenshot_12" src="https://github.com/user-attachments/assets/867fd994-277d-48d3-9862-acdf870faf50" />
<img width="870" height="797" alt="Screenshot_13" src="https://github.com/user-attachments/assets/84a8713e-7925-43bf-ab6e-aea55963e724" />

Inside their webpage, I'll go to _DVWA_, then we'll simulate two logins attempt. The first  one with the wrong credentials to see if we can capture on _Wireshark packets._ and finally the second one with the correct credentials.

<img width="902" height="671" alt="Screenshot_14" src="https://github.com/user-attachments/assets/e9a7b2e2-0769-4b89-a9ac-822632108107" />
<img width="787" height="601" alt="Screenshot_15" src="https://github.com/user-attachments/assets/835c9db0-e0b9-47a5-bd5e-79d7ad126680" />

By just tracing and following the correct _login packet_ on wireshark things start to get interesting.
<img width="1889" height="712" alt="Screenshot_16" src="https://github.com/user-attachments/assets/16053658-04d5-4261-b77d-5398c69fc7b7" />

As mentioned previously, this attempt failed because the credentials were wrong on purpose.
Now what if the credentials were correct? Let's check:

<img width="725" height="708" alt="Screenshot_17" src="https://github.com/user-attachments/assets/d08d95df-bcee-46a9-a68e-69923de172af" />
<img width="938" height="858" alt="Screenshot_18" src="https://github.com/user-attachments/assets/6de4dc7e-7325-42c4-8e39-71f1d1228b8f" />

Back on Wireshark, we follow the _new login packet_ and now have access to the correct credentials.
<img width="1854" height="812" alt="Screenshot_19" src="https://github.com/user-attachments/assets/6c60e51b-898b-4708-b43b-3700aa820e96" />

With the priviledged login in hands, the possibilities of exploitation are endless - such as silently steal their data overtime, but for now I'll just turn their security to the lowest setting possible for demonstration purposes:
<img width="946" height="790" alt="Screenshot_20" src="https://github.com/user-attachments/assets/9e944357-a2b4-40bb-adbf-ec5d51e8898d" />

# Bonus chapter (Parallel attack)

There's another known tool for Kali that would have the same outcome above called the _Set Tool Kit_.
Again, this series are for educational purposes only so do your own due diligence.

Let's quickly check how that works in this scenario:
<img width="562" height="491" alt="Screenshot_21" src="https://github.com/user-attachments/assets/d9664480-4e31-4406-80d2-a9203f6ad606" />

From here you can just follow the prompt.

<img width="480" height="447" alt="Screenshot_22" src="https://github.com/user-attachments/assets/e48d15ef-033b-4ecf-952b-cfbad29f7f14" />
<img width="692" height="700" alt="Screenshot_23" src="https://github.com/user-attachments/assets/03f2e088-bc04-4f12-91cd-4f1d85a86405" />
<img width="647" height="392" alt="Screenshot_24" src="https://github.com/user-attachments/assets/b31c472d-3532-4008-9216-f82b528a5a1e" />
<img width="651" height="617" alt="Screenshot_25" src="https://github.com/user-attachments/assets/a4d7f09d-971a-4d5a-a4a2-e433d307965a" />
<img width="538" height="451" alt="Screenshot_26" src="https://github.com/user-attachments/assets/5abb2edc-dc33-4327-bbd7-09f3369a8238" />
<img width="1647" height="884" alt="Screenshot_27" src="https://github.com/user-attachments/assets/2b6d216a-7496-46c3-9770-b576530dfaf0" />
<img width="463" height="613" alt="Screenshot_28" src="https://github.com/user-attachments/assets/92133b9d-f6a2-441f-8509-f6aee0a0d16b" />
<img width="1621" height="875" alt="Screenshot_29" src="https://github.com/user-attachments/assets/cf45a26b-b903-461f-8eef-ec39158cb247" />






