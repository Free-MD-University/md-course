
# create user : 
curl -X POST "http://localhost:9200/_security/user/{username}" -H "Content-Type: application/json" -u elastic:{elastic pass} -d'
{
  "password" : "{pass}",
  "roles" : [ "superuser" ],
  "full_name" : "{full name}",
  "email" : "acher.klein@vo2-group.com",
  "enabled" : true
}
'


# change password kibana user : 
curl -X POST "http://localhost:9200/_security/user/kibana/_password" \
  -H "Content-Type: application/json" \
  -u elastic:{elastic_pass} \
  -d '{
    "password": "{kibana new pass}"
  }'