

## Start SAS Viya servers and services manually in this sequence:

1. Check and remove the remained processes or zombie service

2. Start the SAS Configuration Server (Consul) 

```bash
systemctl status sas-viya-consul-default 
systemctl start sas-viya-consul-default
systemctl status sas-viya-consul-default 
```

3. Start all instances of SAS Secret Manager (Vault)

```bash
systemctl status sas-viya-vault-default
systemctl start sas-viya-vault-default
systemctl status sas-viya-vault-default
```

4. Start SAS Message Broker (RabbitMQ)

```bash
systemctl status sas-viya-rabbitmq-server-default
systemctl start sas-viya-rabbitmq-server-default
systemctl status sas-viya-rabbitmq-server-default
```

5. Start the SAS Infrastructure Data Server cluster. If you want to use the individual scripts, first start the data nodes and then start the PGPool nodes.

```bash
systemctl start sas-viya-sasdatasvrc-postgres-pgpool0-consul-template-operation_node
systemctl status sas-viya-sasdatasvrc-postgres-pgpool0-consul-template-operation_node

systemctl start sas-viya-sasdatasvrc-postgres-pgpool0-consul-template-pool_hba
systemctl status sas-viya-sasdatasvrc-postgres-pgpool0-consul-template-pool_hba

systemctl start sas-viya-sasdatasvrc-postgres-node0
systemctl status sas-viya-sasdatasvrc-postgres-node0

systemctl start sas-viya-sasdatasvrc-postgres-node0-consul-template-operation_node
systemctl status sas-viya-sasdatasvrc-postgres-node0-consul-template-operation_node

systemctl start sas-viya-sasdatasvrc-postgres-node0-consul-template-pg_hba
systemctl status sas-viya-sasdatasvrc-postgres-node0-consul-template-pg_hba

systemctl start sas-viya-sasdatasvrc-postgres-pgpool0
systemctl status sas-viya-sasdatasvrc-postgres-pgpool0
```

6. Start the HTTP proxy server (Apache HTTP Server)

```bash
systemctl start sas-viya-httpproxy-default
systemctl status sas-viya-httpproxy-default
```

7. Start all remaining services

```bash
./sas-viya-all-services start
./sas-viya-all-services status
```



Reference: [Start and Stop Servers and Services](https://go.documentation.sas.com/doc/en/calcdc/3.5/calchkadm/n00003ongoingtasks00000admin.htm?homeOnFail)

