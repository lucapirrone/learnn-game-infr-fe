# Infrastruttura ambiente FrontEnd
Questo progetto sviluppato con "Serverless Framework" descrive l'infrastruttura 
dell'ambiente AWS su cui viene rilasciato il FrontEnd della web app.

# Stack infrastruttura
L'infrastruttura è composta dal servizio AWS S3, un servizio di storage che offre 
scalabilità, disponibilità continua dei dati, sicurezza e prestazioni all'avanguardia
Il bucket S3 contenente il FrontEnd è impostato in modo tale da hostare 
un sito web statico pubblico (in sola lettura).

# Installazione Serverless Framework
Installa la CLI di serverless framwork tramite npm:
```bash
npm install -g serverless
```

# Deploy
Per il deploy del progetto su AWS viene utilizzata la CLI di serverless framework tramite il comando:
```bash
sls deploy --stage [STAGE]
```
Sostituendo a [STAGE] il valore dello stage su cui si sta lavorando (dev, prod).
In questo modo su AWS si dividono i due stage dell'infrastruttura che a loro volta
conterranno i due stage differenti di FrontEnd:
- Nello stage 'dev' dell'architettura serverless verrà rilasciata la webapp in fase di sviluppo
- Nello stage 'prod' dell'architettura serverless verrà rilasciata la webapp in fase di produzione che utilizzerà il cliente

# Continuous Delivery
Per il CD è impostata un'automazione su seed.run che effettua il deploy del progetto
su AWS in base al ramo git su cui viene effettuato il push:
- Se viene effettuato un push sul ramo git 'main', seed.run automaticamente avvia 
il processo di deploy su AWS nell'ambiente di sviluppo tramite il comando 
```bash
sls deploy --stage dev
```
- Se viene effettuato un push sul ramo git 'master', seed.run automaticamente avvia 
il processo di deploy su AWS nell'ambiente di produzione tramite il comando 
```bash
sls deploy --stage prod
```
In questo modo si hanno due ambienti sepatari dell'infrastruttura che conterranno rispettivamente i due ambienti
della webapp (ambiente di sviluppo e ambiente di produzione).

# Continuous Delivery progetto FrontEnd (learnn-game project) (GitHub Action)
Nella repository del progetto FrontEnd è impostata una action che in base al
ramo su cui si sta pushando una versione, effettua il deploy del progetto sul relativo
stage dell'ambiente AWS:
- In caso di push sul ramo git 'main' viene effettuato il deploy del progetto 
sull'ambiente AWS di sviluppo (stage 'dev')
- In caso di push sul ramo git 'prod' viene effettuato il deploy del progetto
sull'ambiente AWS di produzione (stage 'prod')
