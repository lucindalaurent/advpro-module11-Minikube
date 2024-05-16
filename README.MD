## Reflection 1
1. Compare the application logs before and after you exposed it as a Service. <br>
Try to open the app several times while the proxy into the Service is running. <br>
What do you see in the logs? Does the number of logs increase each time you open the app?<br>

Tampilan log sebelum service di-expose<br>
```
D:\minikube>kubectl logs hello-node-55fdcd95bf-2cwjk
I0516 14:25:19.728813       1 log.go:195] Started HTTP server on port 8080
I0516 14:25:19.730226       1 log.go:195] Started UDP server on port  8081
```
<br>
Tampilan log setelah aplikasi dijalankan sekali

```
I0516 14:25:19.728813       1 log.go:195] Started HTTP server on port 8080
I0516 14:25:19.730226       1 log.go:195] Started UDP server on port  8081
I0516 14:59:41.176120       1 log.go:195] GET /
I0516 14:59:41.252244       1 log.go:195] GET /
```

<br>
Tampilan log setelah aplikasi dijalankan dua kali

```
I0516 14:25:19.728813       1 log.go:195] Started HTTP server on port 8080
I0516 14:25:19.730226       1 log.go:195] Started UDP server on port  8081
I0516 14:59:41.176120       1 log.go:195] GET /
I0516 14:59:41.252244       1 log.go:195] GET /
I0516 15:01:15.364665       1 log.go:195] GET /
I0516 15:01:15.486157       1 log.go:195] GET /
```
<br>
Iya, jumlah log meningkat setiap aplikasi dibuka/dijalankan. Log merupakan catatan dari aktivitas yang terjadi dalam sebuah kontainer pada suatu waktu tertentu. Service yang telah di-expose dapat menerima request. Oleh karena itu log akan mencatat request-request yang dibuat setelah service di-expose.
<br>
<br>

2.  Notice that there are two versions of `kubectl get` invocation during this tutorial section.<br>
The first does not have any option, while the latter has `-n` option with value set to `kube-system`. <br>
What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created? <br>
Jawab: <br>
Opsi '-n' pada `kubectl get` digunakan untuk merinci namespace dari resource Kubernetes yang ingin didapatkan/dilist. Saat kita menjalankan `kubectl get pods`, hasilnya adalah list semua pod di `default` namespace. Sedangkan pada `kubectl get pods -n kube-system`, kita secara eksplisit merujuk ke namespace `kube-system`, sehingga hasilnya hanya list pod pada namespace `kube-system` saja. Alasan kenapa output `kubectl get pods -n kube-system` tidak menunjukkan service `hello-node` yang telah dibuat sebelumnya adalah karena kita meng-expose service tersebut di namespace `default`, bukan di `kube-system`. 