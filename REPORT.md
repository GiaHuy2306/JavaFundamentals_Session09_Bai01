# BÁO CÁO BÀI TẬP 1: KIỂM TRA CÀI ĐẶT DOCKER ENGINE VÀ CHẠY CONTAINER HELLO-WORLD

- **Môn học / Chuyên đề**: DevOps & Java Fundamental
- **Session**: 09 - Docker Cơ Bản & Containerization
- **Bài tập**: Bài 1 - Kiểm tra cài đặt Docker Engine và chạy Container Hello-World
- **Đường dẫn nộp bài**: `homework/session_09/ex1/REPORT.md`

---

## 1. Mục tiêu & Chuẩn bị môi trường

### 1.1. Mục tiêu
- Kiểm tra trạng thái hoạt động của Docker CLI và kết nối đến Docker Engine / Daemon.
- Cấu hình phân quyền cho non-root user thực thi các lệnh Docker mà không cần tiền tố `sudo`.
- Chạy container đầu tiên (`hello-world`) từ Docker Hub để kiểm chứng hệ thống containerization hoạt động chuẩn xác.

### 1.2. Cấu hình phân quyền không dùng `sudo`
Theo hướng dẫn, để người dùng không cần gõ `sudo` trước mỗi lệnh docker, user được thêm vào nhóm `docker`:
```bash
sudo usermod -aG docker $USER && newgrp docker
```
Kiểm tra nhóm của user:
```bash
groups
```
**Output:**
```text
huytuan adm cdrom sudo dip plugdev users docker
```

---

## 2. Kết quả thực thi các lệnh CLI cơ bản

### 2.1. Lệnh 1: `docker version`
Kiểm tra chi tiết phiên bản Client (CLI) và Server (Docker Engine/Daemon).

```bash
docker version
```

**Output Terminal:**
```text
Client:
 Version:           29.1.3
 API version:       1.52
 Go version:        go1.24.13
 Git commit:        29.1.3-0ubuntu4.1
 Built:             Wed Apr 29 16:40:20 2026
 OS/Arch:           linux/amd64
 Context:           default

Server:
 Engine:
  Version:          29.1.3
  API version:      1.52 (minimum version 1.44)
  Go version:       go1.24.13
  Git commit:       29.1.3-0ubuntu4.1
  Built:            Wed Apr 29 16:40:20 2026
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          2.2.2
  GitCommit:        
 runc:
  Version:          1.4.0-0ubuntu1
  GitCommit:        
 docker-init:
  Version:          0.19.0
  GitCommit:        
```

---

### 2.2. Lệnh 2: `docker info`
Hiển thị thông tin tổng quan về hệ thống Docker, cấu hình runtime, storage driver, số lượng container, tài nguyên CPU và RAM.

```bash
docker info
```

**Output Terminal:**
```text
Client:
 Version:    29.1.3
 Context:    default
 Debug Mode: false
 Plugins:
  trust: Manage trust on Docker images (Docker Inc.)
    Version:  29.1.3
    Path:     /usr/libexec/docker/cli-plugins/docker-trust

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 29.1.3
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: json-file
 Cgroup Driver: systemd
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 
 runc version: 
 init version: 
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.6.114.1-microsoft-standard-WSL2
 Operating System: Ubuntu 26.04.1 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 20
 Total Memory: 11.54GiB
 Name: HuyTuan
 ID: 55fda81a-8c97-4bf9-85d1-13cba3ccc7c3
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Firewall Backend: iptables
```

---

### 2.3. Lệnh 3: `docker run hello-world`
Khởi chạy container kiểm tra từ image chính thức `hello-world` trên Docker Hub.

```bash
docker run hello-world
```

**Output Terminal:**
```text
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pulling fs layer
4f55086f7dd0: Download complete
4f55086f7dd0: Pull complete
d5e71e642bf5: Download complete
Digest: sha256:5e23090353324d887c48ad5e5c56d294eab81588df9605b07d1afe895f9cc8f8
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

---

## 3. Đánh giá & Kiểm tra kết quả mong đợi

### 3.1. Đối chiếu kết quả yêu cầu
- **Lệnh kiểm tra**: `docker run hello-world`
- **Kết quả xuất hiện trong Terminal**:
  > **“Hello from Docker! This message shows that your installation appears to be working correctly.”**
- **Đạt yêu cầu**: Toàn bộ chu trình từ kéo image (pull), tạo container (create), chạy ứng dụng (run) và in thông điệp ra chuẩn đầu ra (stdout) đều thành công 100%.

### 3.2. Phân tích cơ chế hoạt động của Docker
Theo 4 bước chuẩn được ghi nhận trong output:
1. **Liên lạc (Contact)**: Docker Client giao tiếp với Docker Daemon qua socket IPC `/var/run/docker.sock`.
2. **Kéo Image (Pull)**: Docker Daemon phát hiện image `hello-world:latest` chưa có ở local cache, tự động kết nối đến Docker Hub Registry để tải về.
3. **Khởi tạo Container (Create & Run)**: Docker Daemon ra lệnh cho `containerd` và `runc` cô lập tài nguyên, tạo namespace, cgroup và thực thi binary bên trong container.
4. **Truyền luồng I/O (Stream)**: Docker Daemon điều hướng luồng xuất chuẩn (stdout) từ container trả về cho Docker Client hiển thị trực tiếp lên Terminal của người dùng.

---

## 4. Kết luận
- Docker Engine và Docker CLI đã được cài đặt và vận hành hoàn hảo.
- Docker Daemon đã phân quyền thành công cho người dùng thông thường, không yêu cầu tiền tố `sudo`.
- Containerization stack (`dockerd`, `containerd`, `runc`) đã sẵn sàng phục vụ các bài thực hành đóng gói và triển khai ứng dụng tiếp theo.
