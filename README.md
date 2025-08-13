# 🗄 Data platform demo – Batch + ML (Cloud-Ready)

## 🎯 Obiettivo
Costruire una **data platform completa in locale** che simuli un caso reale di Data Engineering, includendo:
- Ingestion dati
- Orchestrazione workflow
- Trasformazioni SQL modulari
- Visualizzazione KPI e dashboard
- Machine Learning batch (con predizioni salvate)

Il progetto è sviluppato **100% on-prem** con tecnologie open source, ma è strutturato per essere facilmente **migrabile su cloud** (AWS/GCP/Azure, Snowflake, ClickHouse Cloud).

---

## 🛠 Stack Tecnologico

### **Orchestrazione**
- **Apache Airflow** → gestione DAG per ingestion, trasformazione, ML, schedulazioni e dipendenze.

### **Trasformazioni**
- **dbt Core** (+ adapter `dbt-clickhouse`) → modelli SQL modulari, test di qualità, lineage e documentazione.

### **Data Warehouse**
- **ClickHouse** (Docker) → database OLAP ad alte prestazioni, ideale per query analitiche e time-series.

### **Visualizzazione**
- **Metabase** → dashboard KPI, filtri interattivi e grafici.

### **Machine Learning**
- **Python + scikit-learn** → anomaly detection o forecasting su dati storici.
- *(Opz.)* **MLflow** → tracciamento esperimenti e versionamento modelli.

### **(Opz.) Data Lake**
- **MinIO** (compatibile S3) → simulazione storage oggetti (per formati Parquet/CSV).

### **(Opz.) Streaming**
- **Apache Kafka** + **Apache Flink** → processamento dati in real-time con sink in ClickHouse.
- **Grafana** → monitoring real-time.

---

## 📂 Struttura del Progetto

```
data-platform-demo/
  airflow/          # DAGs e requirements Airflow
  dbt/              # Modelli dbt (stg, int, feat, dim, fact)
  scripts/          # Script Python per ingestion/generazione dati e ML
  storage/          # Dati statici CSV/Parquet
  docs/             # Diagrammi architettura, screenshot UI
  docker-compose.yml
  .env.example
  README.md
```

---

## 🔄 Flusso Dati (Versione Batch)

1. **Ingestion**
   - Airflow scarica dati (da API pubbliche o generatori sintetici).
   - Salva in formato Parquet/CSV in `storage/raw` e carica in ClickHouse (tabelle raw).

2. **Trasformazione**
   - dbt trasforma i dati in:
     - **stg_*** → pulizia e normalizzazione
     - **int_*** → arricchimenti e join
     - **feat_*** → feature engineering per ML
     - **fact_*** / **dim_*** → layer analitico

3. **Machine Learning**
   - Airflow esegue un task Python di training modello.
   - Le predizioni vengono salvate in ClickHouse in una tabella `predictions_*`.

4. **Visualizzazione**
   - Metabase legge da ClickHouse e mostra:
     - KPI principali
     - Serie temporali
     - Distribuzioni predizioni

---

## 📌 Deliverable Finali
- **Codice & Config** → repo GitHub con `docker-compose.yml` e codice pronto.
- **DAG Airflow** → ingestion, trasformazione, ML.
- **Modelli dbt** → modulari, testati, documentati (`dbt docs`).
- **Dashboard Metabase** → KPI + grafici.
- **Modello ML** → addestrato e salvato con predizioni archiviate.
- **Documentazione** → README con diagramma architettura e istruzioni.

---

## 🚀 Avvio Rapido

### 1. Clonare il repository
```bash
git clone https://github.com/aleferr-97/data-platform-demo.git
cd de-data-platform-demo
```

### 2. Configurare variabili ambiente
```bash
cp .env.example .env
```
Modifica `.env` con le tue configurazioni (host, porte, credenziali).

### 3. Avviare l'ambiente
```bash
docker compose up -d
```

### 4. Accedere ai servizi
- Airflow → [http://localhost:8080](http://localhost:8080) (admin / admin)
- ClickHouse → [http://localhost:8123](http://localhost:8123) (HTTP UI)
- Metabase → [http://localhost:3000](http://localhost:3000)

---

## 📈 Roadmap di sviluppo

- [x] Setup Docker Compose con Airflow, ClickHouse, Metabase
- [ ] Creazione dataset mock / API ingestion
- [ ] DAG Airflow per ingestion e caricamento in ClickHouse
- [ ] Modelli dbt (stg → fact) con test e documentazione
- [ ] Dashboard Metabase
- [ ] Modello ML batch con predizioni salvate
- [ ] *(Opz.)* Streaming con Kafka + Flink + Grafana
- [ ] Ottimizzazione performance + cleanup finale

---

## 🗺 Architettura


---

## 🔒 Portabilità Cloud
Il progetto è pensato per essere migrato facilmente su cloud:
- **Airflow** → AWS MWAA, GCP Composer, Astronomer
- **dbt** → Snowflake, BigQuery, Redshift, ClickHouse Cloud
- **ClickHouse** → ClickHouse Cloud / alternative OLAP
- **MinIO** → S3, GCS
- Configurazioni gestite via `.env` e `profiles.yml` dbt con target multipli

---

## 📚 Riferimenti
- [ClickHouse Docs](https://clickhouse.com/docs/)
- [Apache Airflow Docs](https://airflow.apache.org/docs/)
- [dbt Docs](https://docs.getdbt.com/)
- [Metabase Docs](https://www.metabase.com/docs/)
- [MLflow Docs](https://mlflow.org/docs/)
