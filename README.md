# Docker

- Vad är Docker?
- Hur installerar man Docker?
- Vad är DockerHub?
- Demo inklusive *push* till DockerHub?

#### Vad är Docker?

Docker är en Plattform i syfte att köra och hantera **Containers**.

Så vad är Containers?

I tal från Paris 2013(https://www.youtube.com/watch?v=3Kn6_b-1mK4&ab_channel=IIEC_connect) berättar en av skaparna av Docker, Salomon Hykes, om varför man skapade Docker, och vilka problem användandet av Containers var tänkt att lösa.

Han berättar om dåvarande problem med shipping, att flytta en applikation från ett ställe till ett annat. Lösning, åtminstone i en metaforisk mening, hittade man när man började kolla på hur fysiska varor transporteras till sjös, nämligen lagrade i Containers.

Genom att transportera gods i containers med standardiserade mått blir *shipping* väldigt smidigt. I vilken hamn i världen ett fartyg med containers än kommer till finns lyftkranar som kan ta hand om leveransen. På samma sätt är Docker Containers tänkt att fungera.

I Dockers värld innehåller en Container alla kod och dependencies som krävs för att köra en applikation. En Docker Container lever endast för att köra applikationer.

###### Images vs Containers

När man är ny i Docker är det två grundläggande koncept kan ta en stund att särskilja: Docker Images och Docker Containers.

En Docker Image är en mall för hur en Docker Container ska köras.

Det går att använda sig av redan färdiga images, men man kan även skapa sina egna. Vi kommer i denna introduktion gå igenom bägge delar, men vi kommer till det lite senare.

Så en Docker Image är alltså en mall för hur en Docker Container ska köras.

Enkelt uttryck så är en container en image **när den körs**.

Vi kan köra flera containers från samma image.

Innan vi går igenom hur det går till så kan vi börja med att installera Docker.

###### Installera Docker på Mac

**1**

https://docs.docker.com/get-docker/

**2**

Välj **Docker Desktop for Mac**

**3**

Välj passande chip alternativ. Installationen ska nu har startar. Öppna "Docker.dmg" när installationen är färdig.

**4**

Följ instruktionerna. Starta appen och välj "sign in". Om ni inte har konto sedan tidigare följ länken för att skapa ett nytt konto.

Om installation har gått bra så har Docker installerat på vår dator. 

###### Hur Docker körs på ens dator

Öppna en terminal och kör

```
docker version
```

```
Client:
 Cloud integration: 1.0.17
 Version:           20.10.8
 API version:       1.41
 Go version:        go1.16.6
 Git commit:        3967b7d
 Built:             Fri Jul 30 19:55:20 2021
 OS/Arch:           darwin/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.8
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.6
  Git commit:       75249d8
  Built:            Fri Jul 30 19:52:10 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.9
  GitCommit:        e25210fe30a0a703442421b0f60afac609f950a3
 runc:
  Version:          1.0.1
  GitCommit:        v1.0.1-0-g4144b63
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

Vi kan nu se att Docker Server, som vi ännu inte har kommit in på, har Linux som operativsystem. Vad som händer när vi kör Docker på vår dator i princip att en Virtuell maskin körs ovanpå ens eget operativsystem. 

För att ge en något mer teknisk förklaring till vad en Container är och hur den körs, så är en Container en **process** , precis som att Spotify, eller Chrome är en process som körs på ens dator.

Vad är viktiga grundelement för en process? 

1. Isolering. Att en process inte påverkar en annan.
2. Resurser. Att en process alltid har tillräckligt med resurser för att kunna köras.

På vår dator är det **Kernel** som löser detta. När vi kör Docker är det Kernell i det operativsystem som vi såg i Docker Server som löser detta för våra containers.

Vad allt detta tekniska betyder för oss i praktiken är att vi kan köra våra Docker Containers på vilken dator som helst, sålänge att Docker körs.

###### Docker Hub

Docker Hub är som Github för Docker Images, där man kan lagra sina egna, och hämta andras Docker images. 

Öppna en terminal och kör

```
docker run hello-world 
```

```bash
[DockerPresentation] docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete
Digest: sha256:393b81f0ea5a98a7335d7ad44be96fe76ca8eb2eaa76950eb8c989ebf2b78ec0
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
```

Låt oss steg för steg gå igenom detta kommando vi precis har kört för att lite mer förstå hur Docker fungerar.

Här kommunicerar Docker Client och Docker Server med varandra. Med **docker** säger vi att vi vill använda Docker Client. **run hello-world** är ett kommando som Docker Client tar och ger till Docker Server vilken betyder att Docker Servers uppgift är att hämta en Docker Image som heter "hello-world" och skapa en container baserad på den och sedan köra den.

Det första Docker Server är att leta efter imagen lokalt. 

```
Unable to find image 'hello-world:latest' locally
```

När den inte hittar den lokalt går den och leter i Docker Hub, vilket är ett repository för Docker Images. Här hittar vi, förutom detta enkla hello world-exempel många officiella images som ni ofta kommer använda, speciellt senare när ni skapar era egna images. 

https://hub.docker.com/

Så när Docker Server väl har hittat imagen kör den containern, vilket i detta exempel endast var att printa ut en enkel text.

Kör vi återigen

```
docker run hello-world
```

Så kommer Docker Server att ha hittat Imagen lokalt.

###### Hantera Containers i Docker Client

För att lista containrar som körs så kör man

```
docker ps
```

Vill man lista alla containrar som någonsin har kört så kör man

```
docker ps --all
```

Vill ni se alla images ni har lokal kört ni docker images.

```
docker images
```

Docker Image hello-world illustrerade ett bra exempel på hur Docker fungerar när det kommer till att hämta en image från Docker Hub och köra den som en container. Men vad hände egentligen med vår Container? Den körde sitt kommando, och sedan gick den i viloläga med status Exited(0).

För att ge ett exempel på en långvårig container kan vi börja med att köra

```
docker run python
```

Som vi ser går Containern i viloläge direkt.

Kör vi

```
docker image inspect python
```

finner vi dess startup commando.

```
"Cmd": ["python3"],
```

Det betyder att Containern kör det kommandot, och när det är klart så går den i viloläge.

Varje Image har:

1. ett filsystem
2. ett default startup command

Vi kan *override:a* ett default startup command och det gör vi genom att helt enkelt skriva det kommandot längts bak

```
docker run python echo Hello world;
```

Men låt oss släppa hello world för nu. Kör istället

```
docker run -it python /bin/bash
```

-it står för, i princip, interaktivt. Att vi får tillgång till bash inuti containern. När vi kör detta kommando är vi alltså inne i containern som körs baserad på Docker Image python.

kör därefter

```
python
```

och

```
a = 3;
print(a);
```

gå ut python mode med:

```
exit()
```

Det här visar en enorm styrka med Docker. Oavsett om ni har python installerat på er dator eller inte så kommer ni att kunna köra python.

Så om ni jobbar med ett python projekt, och ni pushar upp er image till Docker Hub så kan ni försäkra er om att när er kollega laddar ner samma image på sin dator att det då kommer att fungera även på dennes dator, förutsatt att kollegan kör Docker.

Som ett sista exempel gällande Docker Client så kan ni öppna en ny tab i er terminal. Kör

```
docker ps
```

och kopiera CONTAINER ID

```
docker exec -it <CONTAINER ID> /bin/bash
```

Som ni märker kan *gå in i* containrar som körs. Detta kan vara väldigt nyttigt att veta om i felsökningssyfte.

###### Skapa egna Images

Vi skapar egna images genom att skriva en **Dockerfile**.

Såhär *dockerize* vi en Flask applikation:

**Dockerfile**

https://github.com/AlexisFlach/flask-presentation/tree/main/flaskapp

Här har vi en Flask applikation som vi nu vill Dockerize. Tanken är att vi ska skapa en container som innehåll all kod och dependencies som krävs för att kunna köra applikationen.

**1. Skapa en Dockerfile**

En Dockerfile är uppbyggd av flera rader med instruktioner.

Man börjar alltid med en FROM instruktion som beskriver vilken **base image** vår image ska utgå ifrån.

Tänk på vad målet är med vår container. I detta fall vill vi köra en Python applikation, så att starta med en python image, som har allt vi behöver för att köra en python applikation känns rimligt. Ett alternativ skulle tänkas vara att starta med en ubuntu image och därefter ge ytterliggare instruktioner för att installera python, men det känns som sagt som ett onödigt steg.

https://hub.docker.com/_/python

```
FROM python
```

**2. Workdir**

Vi vill nu ska en **Workdir** inne i vår image där all vår kod hamnar, och där vi utgår ifrån i vår image

```
whereami -> <WORKDIR>
```

```
FROM python
WORKDIR /app
```

**3. Kopiera över requirements.txt till imagen**

Vad vi måste veta när vi skapa en egen image är att Docker läser av filen rad för rad. Varför requirements.txt finns är för att lagra våra dependencies. Vi måste installera det som finns i denna fil, och vi gör det genom

```
RUN pip install -r requirements.txt
```

Om vi skippar steget med att kopiera över requirements.txt från lokalt in i imagen, så kommer docker avbryta skapandet och klaga på att den inte hittar filen.

```.
FROM python
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
```

. står för /app som vi alltså har specificerat som WORKDIR

**4. Installera dependencies**

```
FROM python
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
```

**5. Kopiera över all övrig kod från lokalt till imagen**

```
FROM python
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
```

**6. vilket kommando ska köras när containern kör igång?**

För att starta vår applikation lokalt kör vi i terminalen

```
python3 app.py
```

Samma sak gäller i vår container

```
FROM python
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python3", "app.py"]
```

**7. Skapa en image från Dockerfile:en**

Gå till terminalen och skriv in

```
docker build -t <DockerhubAnvNamn>/<AppNamn> .
```

-t namnger imagen. Det är bästa praxis att namge den med först ert användarnamn på Docker Hub följt av applikationsnamnet. punkten står för **build context**. Ni ska då alltså vara inne i samma mapp som där Dockerfilen finns för att det ska funka.

**8. Pusha till Docker Hub**

```
docker push <DockerhubAnvNamn>/<AppNamn>
```

Gå in Docker Hub och se imagen.









