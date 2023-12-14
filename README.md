# RecAD
Recovering Active Directory
													TryHackMe - Recovering Active Directory
													=======================================

Task 2  Immediate Actions - First Response
==========================================

Hakerlərin ən önəmli cəhdi sistemə davamlı giriş əldə etməkdir. Tamamilə bir sistemdən olan təhdid aktyorlarını idarə etmək mürəkkəb və zamanlı bir prosesdir; Buna görə hücum edənə hücum səthini məhdudlaşdırmaq və yəqin ki, güzəşt olmayan infrastrukturu (serverləri) təcrid etmək çox vacibdir. Aşağıda sağalma prosesinə dərin qazma işdən əvvəl həyata keçiriləcək addımların tez bir cədvəli siyahısı.
Daxili proqram "Windows Server Backup" istifadə edərək güzəşt edilmiş reklam serverinin ehtiyat nüsxəsini götürün. Onu Run> Wbadmin.msc vasitəsilə əldə edə bilərsiniz. Təhlilçilər daha sonra ətraflı zərərli proqram və təhlil təhlili üçün ehtiyat nüsxəsini istifadə edəcəklər.
Qeyd: Zəhmət olmasa, əlavə edilmiş VM-də bir ehtiyat nüsxə yaratmağa çalışmayın.

Windows serverinin etibarlı ehtiyat nüsxəsini bərpa edin. Bu bərpa əməliyyatı, etibarlı ehtiyat nüsxəsini yaratdıqdan sonra domenə əlavə edilmiş reklam obyektləri (istifadəçilər, kompüterlər və s.) Kimi bəzi məlumatların itkisi ilə nəticələnəcəkdir.
Şəbəkəni ayırın və istifadəçiyə maneəsiz xidmətlər göstərmək üçün ikinci dərəcəli domen nəzarətçisini aktivləşdirin.
Şəbəkə səviyyəsində hər hansı bir hücum nümunəsini müəyyən etmək üçün bərpa edilmiş reklam serverindən inkişaf etmiş monitorinqi və trafikin trafikin süzülməsini təmin edin.
Bərpa prosesinin (mümkünsə) başa çatana qədər yeni istifadəçi hesablarının, GPO-ların və s. İnformasiya və modifikasiyanı məhdudlaşdırın.


PS C:\Windows\system32> cd C:\Users\Administrator\Desktop\
PS C:\Users\Administrator\Desktop> ls


    Directory: C:\Users\Administrator\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        4/13/2023   6:29 AM                PowerView
d-----        4/13/2023   6:26 AM                Scripts
-a----        4/13/2023   1:06 PM             18 flag.txt


PS C:\Users\Administrator\Desktop> type .\flag.txt
THM{I_CAN_CONNECT}
PS C:\Users\Administrator\Desktop>


What type of backups can be obtained from the Windows Server Backup utility (write the correct option only)? A: One-time B: Incremental C: Both A and B.
Answer: C

How would you launch the Windows Server Backup utility through the Run dialog box?
Answer: wbadmin.msc


Is it good practice to isolate the infected network infrastructure for detailed network monitoring? (yea/nay).
Answer: yea



Task 3  How did it happen? Identifying Attack Pattern
=====================================================

Hücumun necə baş verdiyini başa düşmək təhlükəyə məruz qalmış maşını bərpa etmək üçün çox vacibdir, çünki hər bir cihaz problemlərin aradan qaldırılmasına və araşdırılmasına kömək edən xüsusi hadisələr əsasında qeydlər hazırlayır. Aşağıda Active Directory kompromis hadisəsini təhlil etmək üçün istifadə olunan vasitələrin siyahısı verilmişdir.

Event Viewer 
------------

Application: Records events of already installed programs.
System: Records events of system components.
Security: Logs events related to security and authentication.

We can access Event Viewer by typing eventvwr in the Run dialog.


BloodHound 
----------

BloodHound iki əsas funksiyası olan Active Directory relyasiya qrafiki alətidir:
O, Active Directory-nin gizli əlaqələrini heyrətamiz şəkildə aşkar edə bilər.
O, Active Directory mühitində hücum yollarını möcüzəvi şəkildə müəyyən edə bilir.
BloodHound-un qiymətləndirilməsi hər hansı bir təşkilat və ya şirkət üçün təhlükəsizlik problemlərinin geniş spektrini üstələmək üçün faydalı ola bilər. Yuxarıda sadalanan əsas funksiyaları yerinə yetirmək üçün qrafik nəzəriyyəsindən istifadə edir. Onun bütün AD kompüterlərindən, qruplarından və istifadəçilərindən məlumat toplamaq üçün qəbuledicisi var (SharpHound adlanır). Bundan əlavə, o, onlayn ehtiyat nüsxələri və yüksək əlçatanlığa malik uzantılar (lisenziyalılar) kimi bir sıra üstünlüklər təklif edir. Əgər alətə ehtiyacınız varsa, onu asanlıqla bu linkdən endirə bilərsiniz. Bu proqram təminatının istifadə etdiyi texnikaları görmək üçün MITRE ATT&CK vebsaytına daxil ola bilərsiniz.

BloodHound - SharpHound Github Resource
https://github.com/BloodHoundAD/BloodHound


MITRE
https://attack.mitre.org/software/S0521/#:~:text=BloodHound%20is%20an%20Active%20Directory,paths%20within%20an%20AD%20environment.

PowerView
---------

https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon

PowerView qırmızı komanda üzvləri tərəfindən Active Directory sadalanması və imtiyazlı hesabların identifikasiyası üçün istifadə edilən məşhur vasitədir. AD-nin sadalanması və ya profilləşdirilməsi hakerlər tərəfindən hücum səthini artırmaq və hədəfin rəqəmsal izini maksimuma çatdırmaq üçün atılan ilk addımdır. Buna görə də, Active Directory-nin təhlükəsizliyinin qiymətləndirilməsi zamanı PowerView AD-nin tam kompromissi ilə nəticələnə biləcək boşluqları müəyyən edir. PowerView PowerShell skriptini bu linkdən endirə bilərsiniz. Linkdə LDAP, domen etibarı, istifadəçi siyahıları və s. üçün müxtəlif PowerShell skriptləri var. 
Əlavə edilmiş VM-də PowerView-i aşağıdakılar vasitəsilə icra edə bilərik:
PowerShell terminalında Import-Module C:\Users\Administrator\Desktop\PowerView\pw.ps1 əmrini icra edin.
Modul idxal edildikdən sonra biz domen nəzarətçisi haqqında məlumat alan Get-NetDomainController kimi müxtəlif əmrləri işlədə bilərik.

PS C:\Windows\system32> Import-Module C:\Users\Administrator\Desktop\PowerView\pw.ps1
PS C:\Windows\system32> Get-NetDomainController


Forest                     : thm.local
CurrentTime                : 12/14/2023 7:00:03 AM
HighestCommittedUsn        : 98351
OSVersion                  : Windows Server 2019 Datacenter
Roles                      : {SchemaRole, NamingRole, PdcRole, RidRole...}
Domain                     : thm.local
IPAddress                  : fe80::1d4e:6db:425c:9993%5
SiteName                   : Default-First-Site-Name
SyncFromAllServersCallback :
InboundConnections         : {}
OutboundConnections        : {}
Name                       : RECOVER.thm.local
Partitions                 : {DC=thm,DC=local, CN=Configuration,DC=thm,DC=local, CN=Schema,CN=Configuration,DC=thm,DC=local, DC=DomainDnsZones,DC=thm,DC=local...}

How many machines in the domain can you find when using PowerView?
Answer: 11


What is the name of the utility in Windows that displays and keeps track of all the events? 
Answer: Event viewer



Task 4  Who did this? Locating an Infection Vector
==================================================

Əgər domen nəzarətçisi təhlükə altına düşərsə, istifadəçilər, kompüterlər və müdaxilələrə əsaslanan qrup siyasətləri kimi domen obyektlərindəki dəyişiklikləri izləyə bilərik' fəaliyyətləri. Adətən, sistemə giriş əldə etdikdən sonra hakerlər əlavə istifadəçilər yaradır, qrup siyasətlərini dəyişdirir və . 


Tracking the changes 
--------------------

Biz istifadəçi icazələrini, obyektlərin yaradılması tarixini (istifadəçi kimi), kompüterlərin qoşulma tarixini, qrup siyasətlərinin dəyişdirilməsini və s. monitorinq etməklə Active Directory-dəki dəyişiklikləri izləyə bilərik. Biz nə ilə bağlı daha ətraflı məlumat əldə etmək üçün Powershell-dən istifadə edəcəyik.

Detection of User(s) Creation/Modifications
-------------------------------------------

Aşağıdakı skript AD istifadəçilərini əldə edəcək' sındırma tarixindən sonra yaradılan hər hansı yeni istifadəçini müəyyən etməyə kömək edəcək yaradılan tarix.


Get-ADUser -Filter {((Enabled -eq $True) -and (Created -gt "Monday, April 10, 2023 00:00:00 AM"))} -Property Created, LastLogonDate | select SamAccountName, Name, Created | Sort-Object Created

PS C:\Windows\system32> Get-ADUser -Filter {((Enabled -eq $True) -and (Created -gt "Monday, April 10, 2023 00:00:00 AM"))} -Property Created, LastLogonDate | select SamAccountName, Name, Created | Sort-Object Created

SamAccountName Name     Created
-------------- ----     -------
evil.guy       evil.guy 4/12/2023 12:19:44 PM

İstifadəçinin evil.guy 12 aprel 2023-cü il tarixində baş vermiş haker hadisəsindən sonra yaradıldığını görə bilərik.

Detection of Computers Joined the Domain
----------------------------------------

Aşağıdakı skriptdə Active Directory domeninə qoşulmuş bütün kompüterlər, ona qoşulmuş şəxsin tarixi və adı göstərilir.

Get-ADComputer -filter * -properties whencreated | Select Name,@{n="Owner";e={(Get-acl "ad:\$($_.distinguishedname)").owner}},whencreated 

PS C:\Windows\system32> Get-ADComputer -filter * -properties whencreated | Select Name,@{n="Owner";e={(Get-acl "ad:\$($_.distinguishedname)").owner}},whencreated

Name        Owner             whencreated
----        -----             -----------
RECOVER     THM\Domain Admins 6/14/2022 12:17:47 AM
SVR-WEB01   THM\Domain Admins 7/13/2022 8:25:19 AM
SRV-DB01    THM\Domain Admins 7/13/2022 8:25:55 AM
SRV-DB02    THM\Domain Admins 7/13/2022 8:26:03 AM
PC-DANIEL   THM\Domain Admins 7/13/2022 8:28:04 AM
PC-MARK     THM\Domain Admins 7/13/2022 8:28:12 AM
LPT-SOPHIE  THM\Domain Admins 7/13/2022 8:28:23 AM
LPT-THOMAS  THM\Domain Admins 7/13/2022 8:28:33 AM
LPT-PHILLIP THM\Domain Admins 7/13/2022 8:28:49 AM
PC-MARY     THM\Domain Admins 7/13/2022 8:28:57 AM
PC-CLAIRE   THM\Domain Admins 7/13/2022 8:29:07 AM


Group Membership Modification 
-----------------------------

Qrup üzvlük dəyişikliklərini yoxlamaq üçün biz hadisə qeydlərinə baxa və müəyyən ssenarilərdə yaradılan xüsusi hadisə identifikatorlarını axtara bilərik. Aşağıda ən maraqlı hadisə identifikatorlarının siyahısı verilmişdir:
ID 4756: Üzv universal təhlükəsizlik qrupuna əlavə edildi.
ID 4757: Üzv universal təhlükəsizlik qrupundan silindi.
ID 4728: Üzv qlobal təhlükəsizlik qrupuna əlavə edildi.
ID 4729: Üzv qlobal təhlükəsizlik qrupundan silindi.
Tədbirləri Run > eventvwr: vasitəsilə əldə edilə bilən Hadisə Baxıcısı vasitəsilə yoxlaya bilərik.

What is the total number of users logged on after Dec 1, 2022?
Answer: 1

What event ID will be logged if a user is removed from a universal security group?
Answer: 4757

What is the email address for the user evil.guy?
Answer: hack@crypto



Task 5  How to get it back? Domain Takeback
===========================================


Təşkilatlarda AD-dən çox istifadə edildiyinə görə, hakerlər həmişə daha az təhlükəsiz bir sistemə güzəştə getməyə çalışırlar. Bu baxımdan, xidmətlərin əlçatanlığını təmin etmək və AD istifadəçiləri üçün dayanma müddətini minimuma endirmək üçün Post-Compromise planı mövcud olmalıdır. Təhlükədən sonra AD-nin bərpası prosesi Domain Geri Alınması adlanır. 

Steps for Recovery Plan
-----------------------

Bu planın bir hissəsi ola biləcək bir neçə vacib şey aşağıdakılardır: 
Tier 0 hesabları üçün parolu sıfırlayın. Sadəcə istədiyiniz seçimi seçməklə hesabı sıfırlaya və ya söndürə bilərsiniz.

Mümkün olan (şübhəli) hesabları axtarın və imtiyazların artmasının qarşısını almaq üçün onların parolunu sıfırlayın.
Kerberos xidmət hesabının parolunu dəyişdirin və onu təcavüzkar üçün yararsız hala salın.
İnzibati imtiyazları olan hesabların parollarını sıfırlayın.
Domendəki kompüter obyektləri üçün sıfırlama əməliyyatlarını yerinə yetirmək üçün Reset-ComputerMachinePassword PowerShell əmrindən istifadə edin.
Gümüş biletdən sui-istifadənin qarşısını almaq üçün domen nəzarətçi maşınının parolunu sıfırlayın. Kerberos əsaslı hücumların müxtəlif növləri haqqında ətraflı öyrənə bilərsiniz burada. 
Domen nəzarətçiləri qorunma və bərpa üçün vacib elementdir. Yazıla bilən domen nəzarətçisini (DC) etibarsız domen üçün ehtiyat nüsxəsi kimi konfiqurasiya etmisinizsə, pozulmanın qarşısını almaq üçün onu bərpa edə bilərsiniz (Bu addımı yerinə yetirərkən diqqətli olun. Təhlükəli DC nümunəsini bərpa etməyin).
Zərərli skriptlərin identifikasiyası üçün istənilən hədəflənmiş domen nəzarətçi serverində zərərli proqram təhlili aparın.
Təcavüzkarın davamlı giriş üçün heç bir planlaşdırılmış tapşırıq və ya işə salma proqramları əlavə etmədiyini yoxlayın. Tapşırıq planlaşdırıcısına Run > taskschd.msc. vasitəsilə daxil ola bilərsiniz


What is the command to perform the password reset operation for a computer in the domain? 
Answer: Reset-ComputerMachinePassword

What is the security vulnerability that involves abusing Kerberos service tickets called?
Answer: silver ticket abuse



Task 6  Why did it happen? Common Misconfigurations
===================================================

Boot source in BIOS
-------------------

Yanlış konfiqurasiya edilmiş BIOS yükləmə sırasına görə, təcavüzkarlar həmçinin serveri pozmaq üçün cihazlarını yükləyə və giriş parollarını dəyişə bilərlər. CD/DVD-dən, xarici cihazlardan (USB) və ya disketdən yükləmənin qarşısını almaq üçün BIOS-u konfiqurasiya edin.

AD Server Administrator Group Members
-------------------------------------

AD Server Administrator Qrupunun Üzvləri
İş stansiyalarına kimin daxil ola biləcəyinə nəzarət AD mühitini sərtləşdirərkən ən böyük problemlərdən biridir. Domain İstifadəçiləri qrupunun bütün üzvləri standart olaraq AD-də istənilən iş stansiyasına daxil ola bilərlər. Buna görə də, imtiyazlı giriş və ya domen nəzarətçisi olan kompüterlərdə yerli olaraq daxil ola bilən istifadəçiləri məhdudlaşdırmaq üçün qabaqlayıcı tədbirlər görməliyik. Bu, "Allow log on locally" 


xüsusi kompüterə daxil ola biləcək xüsusi istifadəçi və ya qrupu seçməyə imkan verən siyasət. Siyasəti aşağıdakı addımlar vasitəsilə aktivləşdirə bilərsiniz:
Çalıştır dialoqunda gpedit.msc yazaraq qrup siyasətinə keçin və sonra Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > User Rights Assignment üzərinə gedin və Allow log on locally



Weak Passwords
--------------

Ətraf mühitə giriş hüququ olmayan hücumçular lüğət və ya kobud güc hücumu vasitəsilə parollar əldə etməklə AD hesablarınızı effektiv şəkildə poza bilər. Təşkilatınızın bu cür hücumlara qarşı nə qədər həssas olduğunu düşünə bilərsiniz. Mimikatz kimi alətlər etimadnamələri çıxarmaq üçün müxtəlif yollar təklif edir; biri DCSync hücumudur. Bu, domen nəzarətçisini təqlid etməyə çalışan və hədəf istifadəçi hesablarının hash kimi parolla əlaqəli məlumatları əldə edə bilən Mimikatz xüsusiyyətidir. Hücumçular istənilən hesabın NTLM parol heşini və ya açıq mətn parolunu əldə edə bilər. Kerberos əsaslı hücumların müxtəlif növləri haqqında burada daha çox öyrənə bilərsiniz. 

Mimikatz - https://tools.thehacker.recipes/mimikatz
dcsync - https://tools.thehacker.recipes/mimikatz/modules/lsadump/dcsync
https://tryhackme.com/room/attackingkerberos

Preventing DCSync Attack
------------------------

DCSync hücumu təcavüzkara domen nəzarətçisini təqlid etməyə və həmin domen nəzarətçisi adından sorğular qəbul etməyə imkan verir. Domendə "permission to replicate information" olan hesabları müəyyən etməklə DCSync hücumunun qarşısı alına bilər. Təcavüzkar hətta domen nəzarətçisinə daxil olmadan hücuma başlaya bilər. Buna görə də, "effective network monitoring"  bu cür hücumları azaltmaq üçün açardır. Əgər DCSync hücumu aşkar etsəniz, təhlükəyə məruz qalmış hesabı dərhal deaktiv etmək təcavüzkar tərəfindən imtiyazların artmasının qarşısını ala bilər və onların Qrup Siyasəti Obyektləri vasitəsilə şəbəkəyə digər dəyişikliklər etmək imkanlarını məhdudlaşdıra bilər.  

Scripts and Applications Permissions on Workstations
----------------------------------------------------

Əgər domen müştərisi icazəsiz proqramları və ya skriptləri işlədə bilirsə, o zaman təcavüzkarlar asanlıqla bütün şəbəkəni sadalaya və hədəf sistemdə aşkar edilən zəifliyə uyğun olaraq istismarları həyata keçirə bilərlər. Bir çox zərərli proqram əmr satırı, PowerShell və toplu faylları istifadə edərək işləyir. Skriptlər və proqramlar üzrə məhdudiyyət siyasəti AD serverində çoxsaylı kiberhücumlardan qoruya bilər. 

Configure Network Time Synchronisation
--------------------------------------

Şəbəkə cihazları və proqramları təhlükəsizlik məlumatı və hadisələrin idarə edilməsi üçün eyni vaxt zonalarından istifadə edə bilməlidir. Təcrübə təhlükə aktorlarının müəyyən edilməsinə və hücum modellərinin əlaqələndirilməsinə kömək edir.


The type of attack that allows attackers to impersonate a domain controller and receive/forward requests on behalf of the domain controller is called?
Answer: DCSync

Is synchronising time on all network devices important to correlate logs on different devices? (yea/nay).
Answer: yea




Task 7  Post Recovery Actions
=============================

Domen nəzarətçisi bərpa edildikdən sonra təcavüzkarların sistemə daxil olmasına imkan verən boşluqları müəyyən etmək üçün hadisəyə tam cavab planı hazırlanmalıdır. Domeni bərpa etdikdən sonra bir neçə vacib hərəkət aşağıda qeyd edilmişdir:

Policy Decisions
----------------

Ətraflı kibertəhlükəsizlik planı NIST kimi bəzi beynəlxalq çərçivələrə uyğun hazırlanmalıdır.
Gələcəkdə belə hücumların qarşısını almaq üçün fəlakətlərin idarə edilməsi siyasətini hazırlayın.
İnsidentin infeksiya vektorunu tapmaq və kök səbəbini müəyyən etmək üçün infrastrukturun ətraflı kibertəhlükəsizlik auditi.
Bütün serverlərdən, kompüterlərdən və şəbəkə cihazlarından qeydlərin saxlanmasını və nüfuzlu SIEM həllinə yönləndirilməsini təmin edin.

NIST - https://www.quest.com/community/blogs/b/microsoft-platform-management/posts/how-to-secure-active-directory-using-the-nist-cybersecurity-framework

Domain Controller
-----------------

Təcavüzkarın istifadə etdiyi komanda və idarəetmə (C2) domenlərini və IP ünvanlarını bloklamaq üçün SIEM-də daimi qaydaların əlavə edilməsi.
İctimaiyyətə açıq olan istismarlar vasitəsilə sistemlərin istismarının qarşısını almaq üçün bütün həssas sistemləri yamaq. 
Bütün domen nəzarətçiləri və domenə qoşulmuş sistemlərin hərtərəfli zərərli proqram skanını həyata keçirin.
Əməliyyat sistemini Windows Server-in ən son versiyasına təkmilləşdirin, çünki o, AES şifrələməsini təmin edir və qırmızı arxitektura dəstəkləyir kimi daha çox təhlükəsizlik xüsusiyyətləri təklif edir. daha səmərəli. 
Domen kontrollerlərindəki fayl paylaşımlarını silin.
Təcavüzkarlar zərərli proqramı bütün şəbəkədə yaya biləcəyi üçün əsas kompüterlərdə çıxarıla bilən medianın istifadəsini deaktiv edin. 

Backups 
-------

Təşkilat şəbəkəsində yüksək əlçatanlıqda lazımsız domen nəzarətçiləri olmalıdır (əsas/ikinci plan).
Avtomatlaşdırılmış ehtiyat və bərpa mexanizmlərini tətbiq edin.
Dürüstlüyün təsdiqlənməsi üçün etibarlı ehtiyat nüsxələrinin müntəzəm olaraq yoxlanılması.  


Implementation of CIS benchmarks
--------------------------------

Kritik sistemlər xarici aləmə məruz qalır və təhlükəsizlik siyasətlərini həyata keçirmək üçün təkmil mexanizmlərə ciddi ehtiyac var. İnternet Təhlükəsizliyi Mərkəzi (MDB) kompüter sistemlərinin təhlükəsizliyini təmin etmək üçün standartları müəyyən edib, onlar endirilə bilər a>əməliyyat sistemindən asılı olaraq. Bu meyarlar istifadəçi və server səviyyələrində lazımi konfiqurasiya dəyişikliklərini təmin edir. Bundan əlavə, bu siyasətlərin sürətlə tətbiqi üçün əvvəlcədən qurulmuş skriptlər onlayn olaraq mövcuddur. Bununla belə, istifadəçi/server səviyyəsində bu cür sərtləşdirməni tətbiq etməzdən əvvəl təşkilatın tələblərini başa düşmək də çox vacibdir. 

