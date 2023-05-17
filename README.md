# Application-chart-template

![Version: 1.0.10](https://img.shields.io/badge/Version-1.0.10-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.0.0](https://img.shields.io/badge/AppVersion-1.0.0-informational?style=flat-square)

A Helm chart template for byzanteam application

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| YangKe | <yangke@jet.work> |  |

## Source Code

* <https://github.com/Byzanteam/application-chart-template/>

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| appIngressroute | list | `[]` |  |
| applicationHosts | list | `[]` |  |
| applicationTLS | object | `{}` |  |
| env | object | `{}` |  |
| externalIngressroute | list | `[]` |  |
| fullnameOverride | string | `""` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"nginx"` |  |
| image.tag | string | `"latest"` |  |
| imageCredentials | object | `{}` |  |
| initContainers | list | `[]` |  |
| nameOverride | string | `""` |  |
| namespace | string | `"default"` |  |
| replicaCount | int | `1` |  |
| restartPolicy | string | `"Always"` |  |
| service.name | string | `"http"` |  |
| service.port | int | `80` |  |
| service.type | string | `"ClusterIP"` |  |
| volumeMounts | list | `[]` |  |
| volumes | list | `[]` |  |

----------------------------------------------

## 支持应用
- jet
- 前端应用
- 后端依赖应用

## Once Helm has been set up correctly, add the repo as follows:

  `helm repo add Byzanteam https://Byzanteam.github.io/application-chart-template`

## 使用前请编写 release.values.yaml 文件

### 1. 设置拉取镜像 name 与 tag
```yaml
image:
  repository: registory/namespace/name
  pullPolicy: IfNotPresent
  tag: "latest"
```
### 2. 设置拉取镜像凭证
```yaml
imageCredentials:
  registry: registry.cn-hangzhou.aliyuncs.com
  username: deploy-man@skylark
  password: changeit
```
### 3. 前端应用设置 application hosts(支持域名和IP)
```yaml
applicationHosts:
  - example.com
  - 192.168.0.1
```
### 4. 前端应用设置反向代理对应 path 规则
```yaml
appIngressroute:
  - name: example-name
    path: /example-path
```
### 5. 前端应用设置外部服务代理规则
```yaml
externalIngressroute:
  - name: "external-name"
    # 外部服务是域名则用 host
    host: "external-host"
    # 外部服务是 ip 则用 ip
    ip: "192.168.0.1"
    port: "443"
    path: "/external-path"
    # 映入外部服务所需要添加的中间件
    middlewares:
      - cors-header
```
### 6. 设置应用需要的 env 参数
```yaml
env: {}
```
### 7. 应用需要使用 https 时设置
> 使用证书挑战设置 `certResolver` 参数
```yaml
applicationTLS:
  certResolver: jet
```
> 使用自有证书设置 `TLSSecret`
```yaml
applicationTLS:
  TLSSecret:
    certificate: certificate-file base64 encoding
    key: key-file base64 encoding
```

## Misc
### 应用启动初始化设置
```yaml
initContainers:
  - name: init
    image: busybox:latest
    command: ["sh", "-c"]
    args: []
    envFrom:
       - configMapRef:
           name: {{ include "application-chart-template.name" $ }}-env
```
### 应用文件挂载设置
```yaml
volumeMounts:
  - mountPath: /path/file
    name: volumeName

volumes:
  - name: volumeName
    hostPath:
      path: /local/path
      type: DirectoryOrCreate
```
