# Overview and notes

For this exercise I'll be running three VM's on an open network adapter.
Note that everything performed here is within a controlled environment set on my own premises for demonstration only purposes.
All tools needed will be listed by the end of the page for reference.

Let's get started.

# Reconnaissance

Also known as exploration, reconnaissance refers to gather information about the network - or victim. For that, we'll first run an _nmap_ command in our linux to explore who else is up in our network.
<img width="705" height="851" alt="Screenshot_1" src="https://github.com/user-attachments/assets/2fe0b30e-a51d-4ab1-8372-fedd599b5417" />

Note that another IP address (192.168.1.210) was found on the same network and all relative ports states.

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

Inside their webpage, I'll go to _DVWA_, we'll then attempt two logins in order to capture the credentials on _Wireshark packets._
<img width="902" height="671" alt="Screenshot_14" src="https://github.com/user-attachments/assets/e9a7b2e2-0769-4b89-a9ac-822632108107" />
<img width="787" height="601" alt="Screenshot_15" src="https://github.com/user-attachments/assets/835c9db0-e0b9-47a5-bd5e-79d7ad126680" />

By just tracing and following the _login packet_ on wireshark things can start to get interesting.
<img width="1889" height="712" alt="Screenshot_16" src="https://github.com/user-attachments/assets/16053658-04d5-4261-b77d-5398c69fc7b7" />

This attempt failed because the credentials were wrong on purpose. Now what if the credentials were correct? Let's see:
<img width="725" height="708" alt="Screenshot_17" src="https://github.com/user-attachments/assets/d08d95df-bcee-46a9-a68e-69923de172af" />
<img width="938" height="858" alt="Screenshot_18" src="https://github.com/user-attachments/assets/6de4dc7e-7325-42c4-8e39-71f1d1228b8f" />

Back on Wireshark, we follow the _new login packet_ again and now we have the correct credentials.
<img width="1854" height="812" alt="Screenshot_19" src="https://github.com/user-attachments/assets/6c60e51b-898b-4708-b43b-3700aa820e96" />






