#-------------------------------
# k8s-rbac-starter
#-------------------------------

kubectl apply -f .
curl -k -X POST https://localhost:31000/grades \
  -H "Content-Type: application/json" \
  -d '{"name": "Harry", "subject": "Defense Against Dark Arts", "score": 95}' -H "Authorization: Bearer $NEW_TOKEN"

curl -k -X POST https://localhost:31000/grades \
  -H "Content-Type: application/json" \
  -d '{"name": "Ron", "subject": "Charms", "score": 82}' -H "Authorization: Bearer $NEW_TOKEN"

curl -k -X POST https://localhost:31000/grades \
  -H "Content-Type: application/json" \
  -d '{"name": "Hermione", "subject": "Potions", "score": 98}' -H "Authorization: Bearer $NEW_TOKEN"

kubectl get secrets grade-sa-token -n grade-demo -o yaml
export NEW_TOKEN=$(kubectl get secret grade-sa-token -n grade-demo -o jsonpath="{.data.token}" | base64 -d)
curl -k https://localhost:31000/grades -H "Authorization: Bearer $NEW_TOKEN"
