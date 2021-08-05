# opc-publisher-k8s
Quick Example showing an OPC Publisher on K8s

## Prereqs
- Azure Cli
- kubectl

## Steps
0. Edit the json file to reflect your desired config.
1. Create NS `kubectl create ns opc`
- (Optional) to save you typing in --namespace opc you can switch your k8s context to be here permanently `kubectl config set-context --current --namespace=opc`
2. Create ConfigMap from the json config file `kubectl create configmap opc-configmap  --from-file=config/pn-opcpubdemo.json --namespace opc`
3. Create and Get the Secret from your IOT Hub Config:
- (Optional) Create the Device: `az iot hub device-identity create --device-id opcpubdemo --hub-name **YOURHUBNAMEHERE**`
- Retrieve the Device Connstring and pipe to file for secret creation: `az iot hub device-identity connection-string show --device-id opcpubdemo --query connectionString --hub-name **YOURHUBNAMEHERE** -o tsv > connstring.txt`
4. Replace MYSECRET with the connstring from above in *secret.yaml*
5. Deploy Secret `kubectl apply -f secret.yaml`
5. Create PVC (Persistent Volume Claim) `kubectl apply -f pvc.yaml`
6. Deploy Pod `kubectl apply -f deployment.yaml`