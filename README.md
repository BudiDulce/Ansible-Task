# Ansible-Task
Contains Frontend.yaml and the docker.yaml files =inventory.ini file
 مشروعي في Ansible على AWS EC2

 ده المشروع بتاعي اللي فيه Ansible Playbooks عشان أتحكم في تلاتة EC2 instances على AWS. المشروع ده بيبين إزاي قدرت أعمل Automation لشويه شغل باستخدام Ansible.

 إيه اللي أنا استخدمته؟

1-AWS EC2: عشان أعمل الـ Virtual Machines (السيرفرات) بتاعتي.
2- Ansible
3-Visual Studio Code (VS Code)
SSH (Secure Shell): عشان أعمل اتصال آمن  جهازي وبين الـ EC2 instances.

 إزاي جهزت الدنيا دي كلها؟

 1. تجهيز الـ EC2 Instances على AWS:

أول حاجة عملتها، رحت على AWS وعملت تلاتة EC2 instances، سميت كل واحدة اسم عشان مَتوهش:
Control-ec2:دي اللي هنزل عليها Ansible وهتحكم منها في باقي السيرفرات.
Front-End-ec2:دي اللي هينزل عليها تطبيق الـ Front-End بتاعي.
Docker-ec2: دي اللي هينزل عليها Docker وهتدير الـ Containers.

 2. إعداد الـ Key Pair (المفتاح السري بتاعي):

عشان أقدر أعمل SSH  مع الـ EC2 instances،  عملتKey Pairعلى AWS وسميته Trial.pem. نزلته عندي على الجهاز عشان أقدر أستخدمه من الـ VS Code عشان أعمل اتصال with the control-ec2.

3. ظبط الـ Security Groups :

دي كانت خطوة مهمة جدًا عشان أضمن إن السيرفرات بتاعتي آمنة، وفي نفس الوقت مسموح لها تتكلم مع بعض:

Security Group للـ Control-ec2:
1- فتحت Port 22 (SSH) عشان أقدر أدخل عليها من الـ IP بتاعي.
 2-  كمان فتحت Port 22 على "Anywhere" (0.0.0.0/0) عشان لو الـ IP بتاعي اتغير، مَتتقطعش مني.

Security Group للـ Front-End-ec2
   1-  فتحت Port 22 (SSH) بس المرة دي، سمحت له بالدخول فقط من الـ Security Group بتاعة الـ Control-ec2. ده معناه إن محدش يقدر يدخل SSH على الـ Front-End غير لو جاي من الـ Control-ec2.
    2-  فتحت كمان Port 80 (HTTP) و Port 443 (HTTPS) على "Anywhere" عشان أي حد يقدر يوصل لتطبيق الـ Front-End بتاعي من المتصفح.

Security Group للـ Docker-ec2 
    1- نفس الفكرة بالظبط، فتحت Port 22 (SSH) فقط من الـ Security Group بتاعة الـ Control-security group.

 4. الاتصا بـ Ansible:

بعد ما ظبطت كل ده، عملت الخطوات دي:

1.  اتصال بـ Control-ec2 : استخدمت VS Code عشان أعمل SSH connection بالـ Control -ec2بتاعتي.
2.  كتابة الـ Playbooks: جوه الـ Control-ec2، كتبت ملفات الـ Ansible Playbooks بتاعتي:
    -front-end.yaml: ده اللي بينزل ويظبط تطبيق الـ Front-End.
    -docker.yaml: ده اللي بينزل Docker وبيجهزه.
3.  إعداد الـ Inventory: عملت ملف inventory.ini وحطيت فيه الـ Public IPs بتاعة الـ Front-End والـ Docker machines، وقسمتهم في "Groups" عشان Ansible يعرف يوصلهم:
[frontend_servers]
frontend_machine ansible_host=54.82.0.61  ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/Trial.pem
[docker_servers]
docker_machine ansible_host=54.174.84.205  ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/Trial.pem
