# Hyperledger Fabric - instruktarz edukacyjny
Jest to krótki instruktarz pokazujący możliwości frameworka blockchainowego [Hyperledger Fabric](https://www.hyperledger.org/projects/fabric) - początkowo rozwijanego przez [Digital Asset](https://digitalasset.com/) i [IBM](https://www.ibm.com/pl-pl), a utrzymywanego obecnie przez [The Linux Fundation](https://www.linuxfoundation.org/) - na przykładzie prostej aplikacji typu [Supply Chain](https://pl.wikipedia.org/wiki/Zarz%C4%85dzanie_%C5%82a%C5%84cuchem_dostaw).

## Spis treści

- [Problem](#problem)
- [Potrzebne materiały](#potrzebne-materiały)
- [Zagadnienia ze świata blockchain](#zagadnienia-ze-świata-blockchain)
  - [Elementy Hyperledger Fabric](#elementy-hyperledger-fabric)
- [Kontekst zadania](#kontekst-zadania)
- [Zadanie praktyczne](#zadanie-praktyczne)

## Problem

Przedmiotem merytorycznym opisywanym w tym materiale jest aplikacja  dostępna w oficjalnym [repozytorium](https://github.com/hyperledger/education) Hyperledger'a, a treść została opracowana na podstawie serii artykułów Mosesa Sam Paula pt. Hyperledger - Blockchain for Business, w której przedstawia on pewien przypadek użycia blockchaina jako jedno z rozwiązań problemu nielegalnego połowu tuńczyków: [Tuna 2020 Traceability Declaration](https://www.weforum.org/agenda/2017/06/tuna-2020-traceability-declaration-stopping-illegal-tuna-from-coming-to-market/).

[Materiał](<https://medium.com/swlh/hyperledger-chapter-5-tuna-fishing-supplychain-context-f593b03e2cf0>) Sam Paula.

## Potrzebne materiały

- [VirtualBox](<https://www.virtualbox.org/>) lub [VMware player](https://www.vmware.com/products/workstation-player.html)
  Manadżer maszyn wirtualnych do zainstancjonowania poniższej.
- [Maszyna wirtualna](http://venstudio.pl/hyperledger-fabric-education/vm.ova) - login: hyper, hasło: hyper
  W celu przyspieszenia procesu inicjalizacji środowiska koniecznego do wypróbowania opisywanej **aplikacji tuńczykowej**, na potrzeby tego instruktarzu została przygotowana maszyna wirtualna oparta o system operacyjny Ubuntu 18 desktop, gotowa do uruchomienia sieci blockchain i aplikacji. 

Powyższa maszyna wirtualna zawiera zainstalowane pakiety:

- [Docker](https://docs.docker.com/)
- [Docker compose](https://docs.docker.com/compose/)
- [Node.js](https://nodejs.org/en/download/) + [NVM](https://www.npmjs.com/package/nvm)
- [Go](https://golang.org/)
- [git](https://git-scm.com/)
- Hyperledger Fabric

## Zagadnienia ze świata blockchain

Moses Sam Paul w bardzo klarowny i dość szczegółowy jak na początek sposób opisał podstawy blockchaina w [tym wpisie](https://medium.com/swlh/hyperledger-chapter-1-foundation-7ad5bd94d452), tutaj natomiast zostały opisane tylko najbardziej kluczowe koncepty.

#### Blockchain - [Wiki](<https://pl.wikipedia.org/wiki/Blockchain>)

"Istotą blockchain jest utrzymanie wspólnej i zbiorowej księgi rachunkowej transakcji, w postaci cyfrowej, rozproszonej po całej sieci, w takich samych kopiach. **Technologia ta opiera się na sieci peer-to-peer bez centralnych komputerów, systemów zarządzających i weryfikujących transakcje**. Każdy komputer w sieci może brać udział w przesyłaniu i uwierzytelnianiu transakcji. W przypadku blockchain będą to bloki w ramach księgi transakcji" ~ Norbert Biedrzycki

#### Distributed Ledger Technology (DLT)

Ledger to księga rachunkowa. A rozproszony ledger to rodzaj struktury danych przechowywany jednocześnie na wielu urządzeniach (node'ach) z reguły rozproszonych w wielu lokalizacjach i regionach.

Ogólnie rzecz biorąc, DLT łączy w sobie technologie blockchainowe i smart kontrakty: 

- **Model danych** zachowuje aktualny stan danych ledgera, tj. seria transakcji chronionych kryptograficznie.
- **Język transakcji** wprowadza możliwość oddziaływania na ledger poprzez odczyty danych i ich zmiana - Smart kontrakty (Chain Code w Hyperledger Fabric)
- **Smart kontrakty** to po prostu progrmay wykonujące predefiniowane akcje pod pewnymi warunkami w   systemie.
- **Protokoły konsensusu** zapewniają spójność danych na wielu rozproszonych urządzeniach jednocześnie. [Przykłady](<https://medium.com/coinmonks/a-primer-on-blockchain-design-89605b287a5a>)
  ![img](https://cdn-images-1.medium.com/max/1080/1*JIClyGovPU7PysqrD3mIoA.png)

### Elementy Hyperledger Fabric

[Oficjalna dokumentajca Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/latest/blockchain.html)

#### Kanały

Kanały to mechanizmy partycjonowania danych, które umożliwiają widoczność transakcji tylko dla interesariuszy. Każdy kanał jest niezależnym łańcuchem bloków transakcji zawierających tylko transakcje dla tego konkretnego kanału.

#### Chain Code, czyli Smart Kontrakt

Zawiera zarówno definicje zasobów, jak i logikę biznesową (lub transakcje) służące do modyfikowania tych zasobów. Wywołania transakcji powodują zmiany w księdze głównej (ledger).

#### Sieć peerów

Sieć to zbiór węzłów przetwarzających dane, tworzące sieć Blockchain. Sieć jest odpowiedzialna za utrzymanie nieustannie replikowanej księgi.

#### **World State**

Stan główny odzwierciedla aktualne dane o wszystkich aktywach w sieci. Dane te są przechowywane w bazie danych w celu optymalnego dostępu. Bieżące obsługiwane bazy danych to LevelDB i CouchDB.

## Kontekst zadania

W celu ograniczenia wpływania na rynek tuńczyków nielegalnego pochodzenia  należy ograniczyć  nielegalny i niekontrolowany połów ryb. Do tego problemu zaprzęgniemy **Hyperledger Fabric** podnoszący transparentność w problemie świata rzeczywistego: **supply chain połowu tuńczyków**.

Będziemy opisywać, w jaki sposób można poprawić efekty połowu tuńczyka u źródła, tj. u rybaczki Sary i procesu, w jakim jej tuńczyk trafia do właścicielki restauracji, Miriam. W międzyczasie zaangażujemy inne strony, np. Regulatora, który weryfikuje jakość połowów tuńczyka.

Prywatny [kanał Hyperledger Fabric](#elementy-hyperledger-fabric) pozwala Sarze i Miriam prywatnie uzgodnić warunki ich handlu, jednocześnie zachowując przejrzystość, aby inni aktorzy mogli jednoznacznie potwierdzić zachodzącą między nimi transakcję.

### Interesariusze

- **Sara** jest rybaczką, który trwale i legalnie łowi tuńczyka.
  Po każdym połowie, Sara zapisuje informacje o każdym pojedynczym tuńczyku, w tym: unikalne ID, miejsce i czas połowu, jego wagę, typ statku i kto złapał rybę. Dla uproszczenia będziemy trzymać się tych sześciu atrybutów.
  Dane zapisywane są w **world state** jako klucz/wartość w ramach smart kontraktu pozwalając Sarze zapisywać dane do ledgera po każdym połowie za pomocą aplikacji, na przykład:

  ```
  var tuna = { id: ‘0001’, holder: ‘Sarah’, location: { latitude: ‘41.40238’, longitude: ‘2.170328’}, when: ‘20170630123546’, weight: ‘58lbs’, vessel : ‘9548E’ }
  ```

- **Regulatorzy** sprawdzają, czy tuńczyk został złowiony zgodnie z prawem.

- **Miriam** jest jest właścicielem restauracji, który w tej sytuacji będzie służyć jako użytkownik końcowy aplikacji mogący odczytywać dane zapisane w ledgerze (księdze głównej transakcji).

## Zadanie praktyczne

### Krok 0 - maszyna wirtualna

Po pobraniu maszyny wirtualnej, należy otworzyć VirtualBoxa (lub VMware, jednak w tym przykładzie posłużę się VirtualBoxem), nastepnie z menu głównego wybrać `File`, a następnie `Import appliance...`. Otworzy się okno importowania obrazu formatu Open VM (*.ovm):

![1557528148338](https://raw.githubusercontent.com/piotr-rozmarynowski/hyperledger-fabric-education/master/images/1557528148338.png)

Zostawiając wszystkie opcje domyślne zaimportuj maszynę.

### Krok 1 - sklonowanie repozytorium

W maszynie wirtualnej uruchom terminal i sklonuj repozytorium, następnie przechodząc do jego katalogu:

```
git clone https://github.com/piotr-rozmarynowski/hyperledger-fabric-education.git
cd hyperledger-fabric-education
```

Struktura katalogów powinna przedstawiać się następująco:

![1557528675745](https://raw.githubusercontent.com/piotr-rozmarynowski/hyperledger-fabric-education/master/images/1557528675745.png)

### Krok 2 - Uruchomienie sieci Fabric'a

Przejdź do katalogu `tuna-app` i uruchom skrypt:

```bash
./startFabric.sh
```

#### Co się dzieje?

Gdy zajrzymy do środka kodu `startFabric.sh`, zauważymy, że wywoływane są: skrypt `../basic-network/start.sh` oraz polecenia dockerowe.

**Docker**

Pobierane są obrazy dockerowe, stawiana jest sieć dla kontenerów.

Za pomocą docker compose instancjonowane są kontenery:

- orderer.example.com
- couchdb
- ca.example.com
- peer0.org1.example.com

```bash
hyper@kryptografia:~/hyperledger-fabric-education/tuna-app$ ./startFabric.sh 

# don't rewrite paths for Windows Git Bash users
export MSYS_NO_PATHCONV=1

docker-compose -f docker-compose.yml down
Removing network net_basic
WARNING: Network net_basic not found.

docker-compose -f docker-compose.yml up -d ca.example.com orderer.example.com peer0.org1.example.com couchdb
Creating network "net_basic" with the default driver
Creating orderer.example.com ... done
Creating ca.example.com ... done
Creating couchdb ... done
Creating peer0.org1.example.com ... 
Creating peer0.org1.example.com ... done
```

#### Start Hyperledger Fabric'a

Tworzony jest kanał dla interesariuszy, peery dołączają do niego, a smart contract (chaincode) jest nań instalowany.

```bash
# wait for Hyperledger Fabric to start
# Create the channel
docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/msp/users/Admin@org1.example.com/msp" peer0.org1.example.com peer channel create -o orderer.example.com:7050 -c mychannel -f /etc/hyperledger/configtx/channel.tx
2019-05-10 22:55:25.217 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2019-05-10 22:55:25.289 UTC [cli.common] readBlock -> INFO 002 Received block: 0
# Join peer0.org1.example.com to the channel.
docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/msp/users/Admin@org1.example.com/msp" peer0.org1.example.com peer channel join -b mychannel.block
2019-05-10 22:55:25.952 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2019-05-10 22:55:26.172 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
Creating cli ... 
Creating cli ... done
2019-05-10 22:55:27.698 UTC [main] InitCmd -> WARN 001 CORE_LOGGING_LEVEL is no longer supported, please use the FABRIC_LOGGING_SPEC environment variable
2019-05-10 22:55:27.714 UTC [main] SetOrdererEnv -> WARN 002 CORE_LOGGING_LEVEL is no longer supported, please use the FABRIC_LOGGING_SPEC environment variable
2019-05-10 22:55:27.728 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2019-05-10 22:55:27.728 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2019-05-10 22:55:28.200 UTC [chaincodeCmd] install -> INFO 005 Installed remotely response:<status:200 payload:"OK" > 
2019-05-10 22:55:28.539 UTC [main] InitCmd -> WARN 001 CORE_LOGGING_LEVEL is no longer supported, please use the FABRIC_LOGGING_SPEC environment variable
2019-05-10 22:55:28.549 UTC [main] SetOrdererEnv -> WARN 002 CORE_LOGGING_LEVEL is no longer supported, please use the FABRIC_LOGGING_SPEC environment variable
2019-05-10 22:55:28.568 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 003 Using default escc
2019-05-10 22:55:28.568 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 004 Using default vscc
2019-05-10 22:56:03.734 UTC [main] InitCmd -> WARN 001 CORE_LOGGING_LEVEL is no longer supported, please use the FABRIC_LOGGING_SPEC environment variable
2019-05-10 22:56:03.749 UTC [main] SetOrdererEnv -> WARN 002 CORE_LOGGING_LEVEL is no longer supported, please use the FABRIC_LOGGING_SPEC environment variable
2019-05-10 22:56:03.759 UTC [chaincodeCmd] chaincodeInvokeOrQuery -> INFO 003 Chaincode invoke successful. result: status:200 

Total execution time : 54 secs ...

Start with the registerAdmin.js, then registerUser.js, then server.js
```

### Krok 3 - rejestracja kont

Aby aplikacja działała poprawnie, musimy zainstalować pakiety Node.js. Zatem, będąc w katalogu `tuna-app` należy uruchomić komendę:

```
npm install
```

##### Rejestracja Administratora

Po poprawnym zainstalowaniu pakietów Node.js, możemy przystąpić do rejestracji konta administratora wywołując komendę:

```
node registerAdmin.js
```

Powinniśmy zobaczyć poniższy output:

```
Store path:/home/hyper/.hfc-key-store
Successfully enrolled admin user "admin"
Assigned the admin user to the fabric client ::{"name":"admin","mspid":"Org1MSP","roles":null,"affiliation":"","enrollmentSecret":"","enrollment":{"signingIdentity":"772ac4b53cb151f67b8739beb3dfc52fdf996122aa437ee149cc34231548f904","identity":{"certificate":"<CERTYFIKAT>"}}}
```

Zaglądając do kodu `registerAdmin.js` można przeczytać, że użytkownik `admin` został zarejestrowany poprzez **Klienta Certificate Authority Fabric'a** . Aby móc przejść dalej i utworzyć nowego użytkownika, należy wysłać tzw. `enroll call` do serwera CA, ponieważ każdy kolejny użytkownik może być rejestrowany tylko, gdy zarejestrowane jest już konto administracyjne.

 ##### Rejestracja `user1`

```
node registerUser.js
```

output:

```
Store path:/home/hyper/.hfc-key-store
Successfully loaded admin from persistence
Successfully registered user1 - secret:VEagLkGNhRaz
Successfully enrolled member user "user1" 
User1 was successfully registered and enrolled and is ready to intreact with the fabric network
```

Ponownie odwołujemy się do serwera CA w celu rejestracji i przypisania nowego użytkownika. `user1` będzie tym, który odczytuje i aktualizuje **ledger**. Utworzenie użytkownika odbędzie się za pośrednictwem `admin`'a, który ma dostęp do serwera CA.

##### Opcjonalny test zależności User1 od Admina

W ramach testu można spróbować usunąć jednostki admina i user1 usuwając ich klucze i certyfikaty z katalogu poprzez `rm -rf ~/.hfc-key-store`. Gdy ponownie wykonamy `node registerUser.js` otrzymamy błąd:

```
Failed to register: Error: Failed to get admin.... run registerAdmin.js
```

jednak zanim uruchomimy `node registerAdmin.js`, należy ponownie wyczyścić wygenerowane certyfikaty (`rm -rf ~/.hfc-key-store`) i dopiero wtedy wykonać skrypty `registerAdmin.js` i `registerUser.js` w takiej kolejności.

**Troubleshooting**

Jeśli pojawia się problem `Registration of 'user1' failed: Identity 'user1' is already registered`, należy wyczyścić (najlepiej wszystkie, aby uniknąć problemów) kontenery poprzez `docker rm -f $(docker ps -aq)`, usunąć wygenerowane poprzednio certyfikaty: `rm -rf ~/.hfc-key-store` i wrócić do kroku 2, jednak już bez instalowania pakietów node.js, gdyż są już pobrane.

### Krok 4 - Uruchomienie aplikacji webowej

```
node server.js
```

Otwórzmy przeglądarkę Firefox, wpisując adres wystartowanej aplikacji webowej: `http://localhost:8000`.

![1557531658239](https://raw.githubusercontent.com/piotr-rozmarynowski/hyperledger-fabric-education/master/images/1557531658239.png)

![1557531702963](https://raw.githubusercontent.com/piotr-rozmarynowski/hyperledger-fabric-education/master/images/1557531702963.png)

### Funkcjonalności

**Dostępne są 4 opcje:**

**Query All Tuna Catches** — uruchamia— `tuna-app/src/queryAllTuna.js`

**Query Specific Tuna** — uruchamia —`tuna-app/src/queryTuna.js`

**Create Tuna Record** — uruchamia— `tuna-app/src/recordTuna.js`

**Change Tuna Holder** — uruchamia —`tuna-app/src/changeTunaHolder.js`

Wszystkie powyższe pliki js mają odwołanie w smart kontrakcie w języku GO: `chaincode/tuna-app/tuna-chaincode.go`. 

#### Dalsze zmiany

W ramach przetestowania technologii zachęcam do samodzielnego dodania opcji usuwania rekordu z ledgera. Będzie do wymagało edycji pliku `controller.js`, dodanie routingu w `routes.js`, utworzenie pliku `tuna-app/src/removeTuna.js` i zapisaniu nowej funkcjonalności w `tuna-chaincode.go`.

