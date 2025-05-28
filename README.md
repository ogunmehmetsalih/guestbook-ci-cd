# Guestbook CI/CD Uygulaması

## uygulama bileşenleri
- Redis Leader: Yazma (write) işlemleri için
- Redis Follower: Okuma (read) işlemleri için
- PHP Frontend: Web arayüzü
- Jenkins: CI/CD otomasyonu
- Ngrok: GitHub webhook’un lokal Jenkins’e ulaşmasını sağlar

## Gereksinimler
- Minikube veya başka bir Kubernetes kümesi
- `kubectl` komutu
- Jenkins (lokal kurulum yeterlidir)
- Ngrok hesabı (ücretsiz)
- GitHub hesabı

## Kubernetes Deployment Adımları

Redis Leader
kubectl apply -f redis-leader-deployment.yaml
kubectl apply -f redis-leader-service.yaml
Redis Follower
kubectl apply -f redis-follower-deployment.yaml
kubectl apply -f redis-follower-service.yaml
Frontendt
kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml
Uygulamayı Görüntüleme
kubectl port-forward svc/frontend 8080:80
Tarayıcıdan ziyaret et:
http://localhost:8080

⚙️ Jenkins ile CI/CD Yapılandırması
🔧 Jenkins Kurulumu
Jenkins’i lokal bilgisayarına kur

Gerekli eklentiler:

Pipeline

Git

GitHub Integration Plugin

🔑 kubectl Erişimi
Jenkins’in çalıştığı kullanıcıda kubectl düzgün çalışmalı:


kubectl get pods
Gerekirse KUBECONFIG ortam değişkenini Jenkins'e tanımla.
GitHub Webhook + Ngrok ile Otomatik Tetikleme
Ngrok Kurulumu
brew install ngrok
ngrok config add-authtoken <your_token>
ngrok http 8080
Ngrok bir adres üretir:
https://abcd1234.ngrok.io → http://localhost:8080
GitHub Webhook Ayarı
GitHub repo > Settings > Webhooks:

Payload URL:
https://abcd1234.ngrok.io/github-webhook/
Content type: application/json

Trigger: Just the push event

⃣Jenkins Pipeline Job Oluştur
Jenkins panel:

New Item > Pipeline > guestbook-pipeline

SCM: Git

Repository URL: https://github.com/kullaniciadi/guestbook-ci-cd.git

Script Path: Jenkinsfile

Trigger: ✅ GitHub hook trigger for GITScm polling
 Test Etme
echo "# test" >> README.md
git add .
git commit -m "Trigger test"
git push origin main
→ Jenkins otomatik çalışır, Kubernetes’e dağıtım yapılır
→ http://localhost:8080 arayüzüne giderek sonucu görebilirsin


