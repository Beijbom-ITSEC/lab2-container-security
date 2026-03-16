# lab2-container-security
lab2-container-security för devsecops kursen

I den här labben har jag skapat en python app med och utan hardning för att jämnaföra resulteten av en trivy scan. För det har jag använt scanningsverktyget som heter Trivy vilket scannar projektet för kända sårbarheter som den ordnar efter hur allvarliga dom är för säkerheten. Den ger även förslag på vilka delar av projektet som behöver uppdateras för att det ska bli mer säkert. 

Hardningen av docker imagen/appen i det här fallet var att ha en uppdaterad version av python, göra användaren till non-root, använde en mindre image för mindre attack yta, healthchecks och en patchad version av flask.
Skillnaden  på storlek blev stor också. Ohärdad image var 1.45GB och efter hardning blev den 206mb.

Sen gjorde jag en SBOM av appen. Det skapar en lista av komponenter som används i appen, t.ex att den använder flask 3.0.0 står i. Med hjäkp av en SBOM kan man bevisa compliance, snabbt hitta nya sårbarheter i komponenterna som används och hjälper till när man ska säkerställa supply chain security för att man kan då lätt hitta vem som ansvarar för de olika komponenterna.

Säkerhet i kuberneter var nästa steg och då använde jag Gatekeepoer och olika policies för att sätta regler på deploymentsen i mitt namespace. I detta fall innebar det att deploymentsen måste t.ex ha labels, måste köras som non-root user, måste finnas begränsningar på hur mycket cpu och minne den använder. Det blir ett extra skydd för att täppa till hål i säkerheten som kan slinga sig förbi när man deployar pods.

Reflection:
Mycket av att säkerställa containers är enkla saker som igentligen inte tar så mycket tid eller energi, speciellt inte när man har byggt upp ett workflow som gör det enkelt. Jag kan ändå se varför det inte alltid görs dock. Man måste utveckla med säkerhet i tanken för att det ska gå smidigt och utvecklare kanske inte ens vet att saker som trivy finns eller hur man skriver en säker Dockerfil. 
SBOM är lika viktigt som att ha innehållsförteckning på ens mat, man måste veta vad som är i för att kunna ta bra beslut. Man måste veta i en program vart alla komponenter kommer ifrån och ifall dom behöver uppdateras. Vid en incident kan man inte bara säga att man inte kollade så noga på vad som var i en program.
Gatekeeper funkar som en portvakt ifall man för någon anledning glömt att fixa små detaljer i sina deployments så att det inte hamnar i produktion.

Screenshots:\
[Tricy scan before hardening](screenshots/trivyscan-before.png) \
[Trivy scan after hardening](screenshots/trivyscan-after.png) \
[Gatekeeper denying a pod](screenshots/gatekeeper-deny.png) \
[Gatekeeper allowing a pod](screenshots/gatekeeper-pass.png) 
