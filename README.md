# Kubernetes Concepts

## 1. Pod
Pod là đơn vị triển khai nhỏ nhất và đơn giản nhất. Một pod đại diện cho một hoặc nhiều container được chạy cùng nhau trên một node, chia sẻ cùng tài nguyên như:
- Network (cùng IP, cổng mạng)
- Storage (volume)

Các container trong một pod thường có quan hệ chặt chẽ và cần giao tiếp với nhau nhanh chóng. Pod được sử dụng để đảm bảo tính nhất quán và hiệu quả khi triển khai ứng dụng trong môi trường Kubernetes.

## 2. ReplicaSet
ReplicaSet là một đối tượng chịu trách nhiệm đảm bảo số lượng pod đang chạy luôn đúng với số lượng mong muốn. Nếu một pod bị lỗi, bị xóa hoặc node bị hỏng, ReplicaSet sẽ tự động tạo ra pod mới để thay thế.

ReplicaSet giúp:
- Đảm bảo tính sẵn sàng cao bằng cách duy trì số lượng bản sao (replica) ổn định của pod
- Tự động phục hồi khi pod bị crash hoặc bị xóa.
- Triển khai mở rộng hoặc thu hẹp ứng dụng bằng cách thay đổi số lượng replica.

ReplicaSet thường được sử dụng gián tiếp thông qua Deployment, giúp dễ dàng cập nhật và quản lý phiên bản ứng dụng.

## 3. Deployment
Deployment là một đối tượng quản lý cấp cao hơn dùng để triển khai và quản lý vòng đời của ReplicaSet và Pod một cách dễ dàng và linh hoạt.

Deployment cho phép:
- Tự động tạo và cập nhật ReplicaSet, từ đó quản lý các pod tương ứng.
- Triển khai phiên bản mới của ứng dụng thông qua cập nhật theo kiểu rolling update mà không gián đoạn dịch vụ.
- Quay lại phiên bản trước nếu có lỗi xảy ra trong quá trình cập nhật.
- Mở rộng hoặc thu hẹp quy mô ứng dụng bằng cách thay đổi số lượng replica.
- Quản lý cấu hình và phiên bản ứng dụng một cách rõ ràng và có thể lặp lại.

Tóm lại, Deployment giúp các nhà phát triển triển khai, cập nhật và duy trì các ứng dụng container hóa trong Kubernetes một cách hiệu quả và an toàn.

## 4. Service
Service là một đối tượng dùng để định danh và truy cập ổn định đến một nhóm Pod đang chạy, bất kể các pod đó có thể thay đổi theo thời gian (do scaling, restart, v.v.).

Một số vai trò chính của Service:
- Cung cấp điểm truy cập ổn định (thông qua tên DNS hoặc IP ảo) cho các pod, dù các pod đó có thể được tạo lại với IP khác.
- Tải cân bằng (load balancing) giữa các pod trong cùng một nhóm (dựa trên label selector).
- Tách biệt logic truy cập và logic triển khai, giúp các thành phần trong hệ thống dễ dàng giao tiếp với nhau mà không cần biết chi tiết hạ tầng.

Các loại Service phổ biến:
1. **ClusterIP** (mặc định): Chỉ truy cập được từ bên trong cluster.
2. **NodePort**: Mở cổng trên các node để truy cập từ bên ngoài.
3. **LoadBalancer**: Dùng với cloud provider để tạo IP public kèm cân bằng tải.
4. **ExternalName**: Gán DNS nội bộ đến tên miền bên ngoài.

## 5. Job
Job là một đối tượng dùng để chạy một hoặc nhiều tác vụ ngắn hạn (batch job) và đảm bảo rằng các tác vụ đó hoàn thành thành công ít nhất một lần.

Khác với Deployment (dùng cho các ứng dụng chạy liên tục), Job dùng cho các tác vụ kết thúc sau khi thực hiện xong, ví dụ như:
- Xử lý dữ liệu một lần (one-time batch processing)
- Tác vụ nhập/xuất dữ liệu
- Gửi email báo cáo định kỳ
- Hoặc chạy migration database.

Chức năng chính của Job:
- Tạo và theo dõi một hoặc nhiều pod cho đến khi tất cả hoàn thành thành công.
- Nếu một pod bị lỗi, Job có thể tạo lại pod mới để thử lại.
- Hỗ trợ parallelism (chạy nhiều pod song song) và completions (số lần hoàn thành cần đạt).

## 6. CronJob
CronJob là một đối tượng dùng để tự động chạy các Job theo lịch định kỳ, tương tự như lệnh cron trong Linux.

CronJob rất hữu ích cho các tác vụ lặp lại theo thời gian, như:
- Sao lưu dữ liệu hàng ngày
- Gửi báo cáo định kỳ
- Dọn dẹp log hoặc dữ liệu tạm
- Đồng bộ dữ liệu theo giờ.

Tính năng chính của CronJob:
- Lên lịch chạy Job dựa trên cú pháp cron (ví dụ: `*/5 * * * *`, `0 0 * * *`, v.v.).
- Tạo Job mới tại thời điểm quy định, và Kubernetes sẽ đảm bảo các Job này được thực thi đầy đủ.

## 7. Ingress
Ingress là một đối tượng dùng để quản lý truy cập từ bên ngoài vào các dịch vụ (Service) bên trong cluster thông qua HTTP và HTTPS.

Nó hoạt động như một reverse proxy giúp định tuyến yêu cầu từ người dùng bên ngoài đến đúng service/pod phía sau, dựa trên host, path, hoặc các quy tắc khác.

Chức năng chính của Ingress:
- Định tuyến HTTP/HTTPS đến các service khác nhau dựa trên tên miền (host) hoặc đường dẫn (path).
- Hỗ trợ SSL/TLS để bảo mật kết nối (HTTPS).
- Tích hợp cân bằng tải (load balancing) cho các request vào.
- Có thể mở rộng với các Ingress Controller như NGINX, Traefik, HAProxy... để xử lý logic điều hướng thực tế.

Ví dụ:
- `example.com/app1` → service A
- `example.com/app2` → service B

Thay vì mở nhiều NodePort hay LoadBalancer, bạn chỉ cần một Ingress duy nhất để định tuyến đến các ứng dụng.

## 8. Secret
Secret là một đối tượng dùng để lưu trữ và quản lý dữ liệu nhạy cảm như:
- Mật khẩu (password)
- Token truy cập
- SSH key
- Chứng chỉ TLS
- Thông tin cấu hình bí mật

Thay vì ghi trực tiếp thông tin nhạy cảm vào file cấu hình hoặc mã nguồn, Secret giúp bảo mật và tách biệt dữ liệu nhạy cảm ra khỏi logic ứng dụng.

## 9. ConfigMap
ConfigMap là một đối tượng dùng để lưu trữ dữ liệu cấu hình không nhạy cảm dưới dạng cặp key-value, giúp tách biệt cấu hình khỏi mã nguồn của ứng dụng.

Mục đích chính:
- Giúp ứng dụng dễ dàng tùy biến hành vi mà không cần rebuild container.
- Cho phép cùng một image ứng dụng có thể chạy trong nhiều môi trường (dev, staging, prod) với cấu hình khác nhau.

## 10. PersistentVolumeClaim (PVC)
PersistentVolumeClaim là một đối tượng do người dùng tạo ra để yêu cầu sử dụng một phần dung lượng lưu trữ (storage) từ hệ thống — thường được dùng để gắn vào các pod như một ổ đĩa bền vững.

Vai trò của PVC:
- Truy cập vào ổ lưu trữ lâu dài (persistent), khác với volume tạm thời bị mất khi pod bị xóa.
- Tách biệt yêu cầu của ứng dụng với hạ tầng lưu trữ (không cần biết chi tiết ổ cứng là gì, ở đâu).
- Tự động liên kết với PersistentVolume (PV) — một tài nguyên do admin tạo sẵn hoặc do hệ thống provision tự động.

## 11. PersistentVolume (PV)
PersistentVolume là một tài nguyên lưu trữ bền vững được cấp sẵn bởi admin hoặc được tạo động thông qua cơ chế dynamic provisioning, nhằm phục vụ cho các ứng dụng yêu cầu lưu trữ lâu dài.

Vai trò của PV:
- Là đại diện cho một ổ lưu trữ vật lý hoặc mạng (VD: NFS, iSCSI, EBS, GCE Disk, v.v.).
- Tồn tại độc lập với vòng đời của pod — dữ liệu sẽ không mất khi pod bị xóa.
- Là đối tượng được "bind" với PVC (PersistentVolumeClaim) để sử dụng trong ứng dụng.

Các thuộc tính chính của PV:
- Capacity: dung lượng lưu trữ (ví dụ: 10Gi).
- Access Modes: kiểu truy cập:
  - ReadWriteOnce (chỉ một node ghi)
  - ReadOnlyMany (nhiều node chỉ đọc)
  - ReadWriteMany (nhiều node có thể ghi)
- Reclaim Policy: chính sách thu hồi sau khi PVC bị xóa:
  - Retain: giữ lại dữ liệu
  - Recycle: xóa sạch nội dung
  - Delete: xóa hoàn toàn volume
- StorageClassName: liên kết đến loại lưu trữ (SSD, HDD, NFS…)

### Mối quan hệ PV – PVC:
- PVC là yêu cầu, PV là tài nguyên đáp ứng.
- Khi PVC được tạo, hệ thống sẽ tìm PV tương ứng (hoặc tạo mới nếu hỗ trợ dynamic provisioning).
- Sau khi được bind, ứng dụng có thể mount PV qua PVC và lưu trữ dữ liệu.

## 12. StorageClass
StorageClass là một đối tượng định nghĩa loại và cách thức cấp phát lưu trữ (storage) cho các PersistentVolume (PV), giúp tự động tạo PV khi một PersistentVolumeClaim (PVC) yêu cầu.

Vai trò của StorageClass:
- Tự động provision (dynamic provisioning): Khi người dùng tạo PVC có chỉ định `storageClassName`, Kubernetes sẽ dựa vào đó để tạo PV tương ứng mà không cần admin tạo thủ công.
- Cho phép đa dạng hóa loại lưu trữ (SSD, HDD, NFS, cloud disk...) trong cùng một cluster.
- Tùy biến cách cấp phát lưu trữ thông qua provisioner, parameters và policies.

Thành phần chính của StorageClass:
- Provisioner: chỉ định driver dùng để tạo PV
- Parameters: cấu hình chi tiết như loại ổ đĩa (type: gp2), vùng (zone: us-east-1a), IOPS...
- ReclaimPolicy: mặc định là Delete hoặc Retain khi PVC bị xóa.
- AllowVolumeExpansion: cho phép mở rộng dung lượng lưu trữ.
- MountOptions: các tùy chọn khi mount volume.

## 13. Namespace
Namespace là một cơ chế giúp phân chia không gian tài nguyên trong cluster, cho phép tổ chức và quản lý các tài nguyên (pod, service, deployment, v.v.) một cách logic và tách biệt.

## 14. Nodes
Node là máy chủ vật lý hoặc máy ảo nơi các Pod được chạy thực tế. Mỗi node đại diện cho một đơn vị tài nguyên trong cluster, bao gồm CPU, RAM, và ổ lưu trữ – những thứ mà Kubernetes sử dụng để triển khai và chạy ứng dụng.

Có hai loại Node:
1. **Master Node (Control Plane)** – quản lý và điều phối cluster:
   - Chạy các thành phần như: kube-apiserver, kube-scheduler, kube-controller-manager, etcd.
   - Không chạy ứng dụng người dùng (trừ khi được cấu hình đặc biệt).
2. **Worker Node** – nơi các Pod của người dùng được triển khai:
   - Chạy các thành phần: kubelet, kube-proxy, và container runtime (Docker, containerd...).
   - Là nơi các container thực sự được thực thi.

Thành phần quan trọng trên mỗi Node:
- kubelet: chịu trách nhiệm tương tác với API server và quản lý pod trên node đó.
- kube-proxy: xử lý networking cho pod, định tuyến request.
- Container runtime: công cụ chạy container như Docker hoặc containerd.

Một số đặc điểm:
- Node có thể tự động được thêm/xóa bởi các công cụ quản lý (auto-scaling).
- Kubernetes sẽ tự động chọn node phù hợp để đặt pod dựa trên tài nguyên sẵn có, nhãn (labels), và ràng buộc.
- Trạng thái node (Ready/NotReady) được cập nhật định kỳ để đảm bảo tính ổn định cho hệ thống.
