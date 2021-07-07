# HomeWork-IS

Домашнее задание по лекции "Элементы безопасности информационных систем"

1. Установите Hashicorp Vault в виртуальной машине Vagrant/VirtualBox

для установки использовал информацию по ссылке дополнительно к официальному сайту

https://itsecforu.ru/2019/03/05/%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-%D0%B8-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-hashicorp-vault-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0-%D0%BD%D0%B0-ubuntu-ce/


vagrant@vagrant:/etc/vault$ systemctl status vault
● vault.service - "HashiCorp Vault - A tool for managing secrets"
     Loaded: loaded (/etc/systemd/system/vault.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2021-07-01 18:07:51 UTC; 15s ago
       Docs: https://www.vaultproject.io/docs/
   Main PID: 13277 (vault)
      Tasks: 6 (limit: 1074)
     Memory: 15.2M
     CGroup: /system.slice/vault.service
             └─13277 /usr/local/bin/vault server -config=/etc/vault/config.hcl

Jul 01 18:07:51 vagrant vault[13277]:               Go Version: go1.15.13
Jul 01 18:07:51 vagrant vault[13277]:               Listener 1: tcp (addr: "0.0.0.0:8200", cluster address: "0.0.0.0:82>
Jul 01 18:07:51 vagrant vault[13277]:                Log Level: info
Jul 01 18:07:51 vagrant vault[13277]:                    Mlock: supported: true, enabled: false
Jul 01 18:07:51 vagrant vault[13277]:            Recovery Mode: false
Jul 01 18:07:51 vagrant vault[13277]:                  Storage: file
Jul 01 18:07:51 vagrant vault[13277]:                  Version: Vault v1.7.3
Jul 01 18:07:51 vagrant vault[13277]:              Version Sha: 5d517c864c8f10385bf65627891bc7ef55f5e827
Jul 01 18:07:51 vagrant vault[13277]: ==> Vault server started! Log data will stream in below:
Jul 01 18:07:51 vagrant vault[13277]: 2021-07-01T18:07:51.358Z [INFO]  proxy environment: http_proxy="" https_proxy="" >
vagrant@vagrant:/etc/vault$


2. Запустить Vault-сервер

Сервер запущен и в веб интерфейс зашёл с помощью ключей


vagrant@vagrant:/etc/vault$
vagrant@vagrant:/etc/vault$ cat /etc/vault/init.file
Unseal Key 1: r9aMoIb1MnPNaFmxzglxoYbaY51Z0VMNp3rv390U/L+a
Unseal Key 2: XcVhWRFhNyzRdOxiYbnfsMNL0RhuSqXUkQXKAhCFFNFR
Unseal Key 3: cVWek8xQ3wE5FC8S7xKNCsJ49qI73hY226F8vXbTQCSO
Unseal Key 4: GrXuIDIQSWKmudOgc/2Tf1zSXrB8B2IGh5aEsycyuqI0
Unseal Key 5: KzXqP78AIexYeo6IprSHu5ttRhnOpePjjWHDf9jh07bT

Initial Root Token: s.iNswbHGFntBCiQ0F59DizefJ

Vault initialized with 5 key shares and a key threshold of 3. Please securely
distribute the key shares printed above. When the Vault is re-sealed,
restarted, or stopped, you must supply at least 3 of these keys to unseal it
before it can start servicing requests.

Vault does not store the generated master key. Without at least 3 key to
reconstruct the master key, Vault will remain permanently sealed!

It is possible to generate new unseal keys, provided you have a quorum of
existing unseal keys shares. See "vault operator rekey" for more information.
vagrant@vagrant:/etc/vault$

Ссылки на скриншоты

https://clip2net.com/s/4czdFD5


https://clip2net.com/s/4czdLph

https://clip2net.com/s/4czdQkR

vagrant@vagrant:/etc/vault$
vagrant@vagrant:/etc/vault$ vault status
Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    5
Threshold       3
Version         1.7.3
Storage Type    file
Cluster Name    vault
Cluster ID      a24ca05a-cd6f-9a66-5f10-31e1989b9903
HA Enabled      false
vagrant@vagrant:/etc/vault$


3. 


vagrant@vagrant:/etc/vault$ vault secrets enable pki
Success! Enabled the pki secrets engine at: pki/
vagrant@vagrant:/etc/vault$ vault secrets tune -max-lease-ttl=8760h pki
Success! Tuned the secrets engine at: pki/
vagrant@vagrant:/etc/vault$

vault write pki/root/generate/internal \
        common_name="olegrovenskiy.com" \
        ttl=87600h



vagrant@vagrant:/etc/vault$
vagrant@vagrant:/etc/vault$ vault write -field=certificate pki/root/generate/internal \
>         common_name="olegrovenskiy.com" \
>         ttl=87600h
-----BEGIN CERTIFICATE-----
MIIDSDCCAjCgAwIBAgIURRogdrXTFfbSysMVOjD7nlcaJ4YwDQYJKoZIhvcNAQEL
BQAwHDEaMBgGA1UEAxMRb2xlZ3JvdmVuc2tpeS5jb20wHhcNMjEwNzAxMTkwMTI2
WhcNMjIwNzAxMTkwMTU2WjAcMRowGAYDVQQDExFvbGVncm92ZW5za2l5LmNvbTCC
ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKvg3DNTJ1nJM4EkiTxPv6AC
fa4zDI+7KqTqAbWMpKmZSYioTsrrN3fKOkac0prDgHHA4CiU+l0XrNmJKx37eF2C
8k1uBIwEc/dp06hHWxhC2MoaoYrrIvZSw+OV8HFikiqUaudJT+7OWYUHXMQsxw7O
ucpWuqF3c87AD9NpzzgWBDUaqhK9rhNEgf8c94BD0zq96pzRmt/+UauEAH3TLoRZ
OA/0z9fA+aVCEE2R4KC0nK9xyz3SWsCZPW3OINLIL9lxbTtC1dwofoasTZWVsxLV
/iTUBm2xD/0dmD+vMooT8hfS6IB+VZPhBd4Tvya43sCg7Tesr/4r+yCH765UGhMC
AwEAAaOBgTB/MA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMBAf8EBTADAQH/MB0GA1Ud
DgQWBBQ3M6UvjDBbCwiGbapFg5Df2NkjXDAfBgNVHSMEGDAWgBQ3M6UvjDBbCwiG
bapFg5Df2NkjXDAcBgNVHREEFTATghFvbGVncm92ZW5za2l5LmNvbTANBgkqhkiG
9w0BAQsFAAOCAQEAlGf9a+HOXAVYTVns2bvneU4jH6VGu9bqavM8YtZoI1FPjCku
McJhYRfiXdzamJ72VpkztfV9j0FVTwIKNbbGB75MTXIhBtBm+I5AGLXwH2Meeeai
1RqRRZBN0yPZuxh0uwVsKFLq6O2iWIDK1eIwmmq6TMzBHhiTLP/xT7e+svVpBUjb
XMDKjUjvWLmK6lvvzpRjAL6xYt2It0TT7h1r9KJQL4T1MHsUSXZTmEI5JkirxL9a
3+fXiEUMAGqp2pV5Q65svRaf3DOpFXV/MNFiJtMGAUVNL02Gf2Nz9wTXX4Pmv+qn
YVxCmdt9QuXWc9Y7CkEN/A8oHExbIklSmtiMjQ==
-----END CERTIFICATE-----
vagrant@vagrant:/etc/vault$

vagrant@vagrant:/etc/vault$ vault write pki/config/urls \
>     issuing_certificates="http://127.0.0.1:8200/v1/pki/ca" \
>     crl_distribution_points="http://127.0.0.1:8200/v1/pki/crl"
Success! Data written to: pki/config/urls
vagrant@vagrant:/etc/vault$

vagrant@vagrant:/etc/vault$ vault secrets enable -path=pki_int pki
Success! Enabled the pki secrets engine at: pki_int/
vagrant@vagrant:/etc/vault$

vagrant@vagrant:/etc/vault$ vault secrets tune -max-lease-ttl=43800h pki_int
Success! Tuned the secrets engine at: pki_int/
vagrant@vagrant:/etc/vault$


vault write pki/roles/example-dot-com \
    allowed_domains=olegrovenskiy.com \
    allow_subdomains=true \
    max_ttl=72h

vagrant@vagrant:/etc/vault$ vault write pki/roles/example-dot-com \
>     allowed_domains=olegrovenskiy.com \
>     allow_subdomains=true \
>     max_ttl=72h
Success! Data written to: pki/roles/example-dot-com
vagrant@vagrant:/etc/vault$


vault write pki/issue/example-dot-com \
    common_name=www.olegrovenskiy.com


vagrant@vagrant:/etc/vault$ vault write pki/issue/example-dot-com \
>     common_name=www.olegrovenskiy.com
Key                 Value
---                 -----
certificate         -----BEGIN CERTIFICATE-----
MIID0TCCArmgAwIBAgIUVdKD1ucUYeVnwT5nrm75cdNUa1YwDQYJKoZIhvcNAQEL
BQAwHDEaMBgGA1UEAxMRb2xlZ3JvdmVuc2tpeS5jb20wHhcNMjEwNzAxMTkxNDAw
WhcNMjEwNzAyMDUxNDMwWjAgMR4wHAYDVQQDExV3d3cub2xlZ3JvdmVuc2tpeS5j
b20wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCuVs3SY1jsjPNZbm5M
BaWjWjAz0uuh/RBYZpLp5tKK+GJ+QqeN8uaSPj+KtqX8Ptiqd9X6O/IayL7dmBqx
4pEQTZILF7W+r6FkISeSJf5To+owCtQl0mJItqJRaqm2KiJMhNrUcjnj9pyjh90z
IvBfOckXQDsen9jbrwY/vIKRbGWkZGCXvvHeoIbEJtFtKbxe78jXlvrghV2BvGx7
1EsPMNHfWWi4aYIf7IxJWR261ZTo4CyuvjGbA7lnX9vnAK+L66HyNYLZAR8KSvtg
V9fogKA2xEc8vPUcBEaFXVvTEDqCdMF9tAG+ZWJI4RxUw9gUClMbCRHE+xfmYots
cGwhAgMBAAGjggEFMIIBATAOBgNVHQ8BAf8EBAMCA6gwHQYDVR0lBBYwFAYIKwYB
BQUHAwEGCCsGAQUFBwMCMB0GA1UdDgQWBBShUSqp9DuWkXSB6jUTGiwEZZ+S5jAf
BgNVHSMEGDAWgBQ3M6UvjDBbCwiGbapFg5Df2NkjXDA7BggrBgEFBQcBAQQvMC0w
KwYIKwYBBQUHMAKGH2h0dHA6Ly8xMjcuMC4wLjE6ODIwMC92MS9wa2kvY2EwIAYD
VR0RBBkwF4IVd3d3Lm9sZWdyb3ZlbnNraXkuY29tMDEGA1UdHwQqMCgwJqAkoCKG
IGh0dHA6Ly8xMjcuMC4wLjE6ODIwMC92MS9wa2kvY3JsMA0GCSqGSIb3DQEBCwUA
A4IBAQCT6U5+7ecV7WuSRyBn1YZbx1JXdbUlRJs2o2L4LAPGdrut+UYw8vnEBmiv
TK5zdRFjQrRkZRJQJjOc+sT+ybly6U78+MNs3j2mHl3ix7hL3GVz2FxaGGMhE9hB
zypCUNtwntbos044J4v9xGLPoZyMmtMc+OvmlZ0wZEEbdfK0lE143jSvbWdKb5T7
f0njKdJT6W1NZfce8eyWXKCLtmtxIDWOiyksSnLbDiERqIfjWLCJ3pv6GO+K/Nl6
Scta/H/hpA3HgQweMHgeq+OmP/mwjnQpFvIOSzU7C8ahNpTkBr9WIqZKAH7w7TmB
J+UtTz+rQ0uPxaxcbx8otLuRInP3
-----END CERTIFICATE-----
expiration          1625202870
issuing_ca          -----BEGIN CERTIFICATE-----
MIIDSDCCAjCgAwIBAgIURRogdrXTFfbSysMVOjD7nlcaJ4YwDQYJKoZIhvcNAQEL
BQAwHDEaMBgGA1UEAxMRb2xlZ3JvdmVuc2tpeS5jb20wHhcNMjEwNzAxMTkwMTI2
WhcNMjIwNzAxMTkwMTU2WjAcMRowGAYDVQQDExFvbGVncm92ZW5za2l5LmNvbTCC
ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKvg3DNTJ1nJM4EkiTxPv6AC
fa4zDI+7KqTqAbWMpKmZSYioTsrrN3fKOkac0prDgHHA4CiU+l0XrNmJKx37eF2C
8k1uBIwEc/dp06hHWxhC2MoaoYrrIvZSw+OV8HFikiqUaudJT+7OWYUHXMQsxw7O
ucpWuqF3c87AD9NpzzgWBDUaqhK9rhNEgf8c94BD0zq96pzRmt/+UauEAH3TLoRZ
OA/0z9fA+aVCEE2R4KC0nK9xyz3SWsCZPW3OINLIL9lxbTtC1dwofoasTZWVsxLV
/iTUBm2xD/0dmD+vMooT8hfS6IB+VZPhBd4Tvya43sCg7Tesr/4r+yCH765UGhMC
AwEAAaOBgTB/MA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMBAf8EBTADAQH/MB0GA1Ud
DgQWBBQ3M6UvjDBbCwiGbapFg5Df2NkjXDAfBgNVHSMEGDAWgBQ3M6UvjDBbCwiG
bapFg5Df2NkjXDAcBgNVHREEFTATghFvbGVncm92ZW5za2l5LmNvbTANBgkqhkiG
9w0BAQsFAAOCAQEAlGf9a+HOXAVYTVns2bvneU4jH6VGu9bqavM8YtZoI1FPjCku
McJhYRfiXdzamJ72VpkztfV9j0FVTwIKNbbGB75MTXIhBtBm+I5AGLXwH2Meeeai
1RqRRZBN0yPZuxh0uwVsKFLq6O2iWIDK1eIwmmq6TMzBHhiTLP/xT7e+svVpBUjb
XMDKjUjvWLmK6lvvzpRjAL6xYt2It0TT7h1r9KJQL4T1MHsUSXZTmEI5JkirxL9a
3+fXiEUMAGqp2pV5Q65svRaf3DOpFXV/MNFiJtMGAUVNL02Gf2Nz9wTXX4Pmv+qn
YVxCmdt9QuXWc9Y7CkEN/A8oHExbIklSmtiMjQ==
-----END CERTIFICATE-----
private_key         -----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEArlbN0mNY7IzzWW5uTAWlo1owM9Lrof0QWGaS6ebSivhifkKn
jfLmkj4/iral/D7YqnfV+jvyGsi+3ZgaseKREE2SCxe1vq+hZCEnkiX+U6PqMArU
JdJiSLaiUWqptioiTITa1HI54/aco4fdMyLwXznJF0A7Hp/Y268GP7yCkWxlpGRg
l77x3qCGxCbRbSm8Xu/I15b64IVdgbxse9RLDzDR31louGmCH+yMSVkdutWU6OAs
rr4xmwO5Z1/b5wCvi+uh8jWC2QEfCkr7YFfX6ICgNsRHPLz1HARGhV1b0xA6gnTB
fbQBvmViSOEcVMPYFApTGwkRxPsX5mKLbHBsIQIDAQABAoIBABHIfvQv+BkhA42i
yxNsHAo+n94ZbLm4U5uA0wmS5vUQAxP3/plnJofSW67tlJ7XVkiFMsl0pex/f6Cg
7FAq2Ts9fmEtSPereJ37F8s7nuavOKsv35YAENBz+LivVaJkR91gS+YRxL/xHuc7
a5/Ut4ovHckGX0FvcrJt9wg3VWY56kRjIXorigFVxssn4+nEJgxCuh8NOJr417ND
J4x+rvQN085mOU4383ospIBSCwBognx2/BoTnXfOBpVLPHqc03FaEZcRD0RvAhkH
kwLAesBUAvQC93RA+SV8zLfrTl8VgraY5qyqtAvgzgZdGnF4i0fe8ZwaRqSP5tSD
R78vWrUCgYEA19Np9e3WNIJuOcrQ2tCL8tO4dXGLhx996Hpda3zgNqojWcqMgB9A
WPR3a1XC+S74lRZPLrqax5D6aUMH6dxczx57tWfJ/CmM20UBwnQMu4/OmxiyH370
UBz7NB6LiWEDB7bAFdhv9dUUOitRJ93SqFcdcqLZr+1jBtRlKQiyce8CgYEAzspz
7k8wOqpJp8/ChrCG+IQGfER8SA1o7f9pT/kV4HBbokX4Oy7YS0kX0SWUGc55+SkE
iy6gCuYsc+WfrhzRhZXu8x9I7ueVaFczi8e90oWMLI9jUnhDNeK824k+38ugmjm9
vGWbBlYeMK/hpRiVoiWJrNixLDKPtBMsNXO+0u8CgYAl55V2gbzbIAUn1Tz1ESdj
EFgXGEf/BULhr4v0ssvWDe+Dd5VRcyuj89t9WGSTyvIRjQd/F3rTjdzM297p7a/H
GH11kLLivJFmeSoj8qnBzzHj/2RZL7zMzSo5Lxwmlokns6rsq0SOkkupI65vKAA1
XIdpeLxur2xy6J6TpFlitQKBgHAEcUpdcSXGSwHxZFGr3GFQu2ajfqNVSErsOXMN
3hDnLGw59N6yYI5fuNwdvB1CoQYcdw6iIiXlS504fQhHKiRv9LGUd+CaaG/OFOka
OTSKDUWyIr5w0Q+mlGFj9kAqwQPqVWJxs8l56v66t8PEDoJ4TRzpgQgNF5UXcbI+
0dbXAoGAebI+aSxvHkWtdzcghew/Ra0k5ABCWFZCgJS38qmeD5aaTNfUs72DN4Pk
HxwsfeuDocuPtJhIhUPfBMceQlL3T01RSAcEElo5lpWxfkhox/BwtSd/WHG7eodr
EEEf0+l5rFSpEdJPQBCwVO5sQADQji0f+U1KIlxrbQeE2cSrBWE=
-----END RSA PRIVATE KEY-----
private_key_type    rsa
serial_number       55:d2:83:d6:e7:14:61:e5:67:c1:3e:67:ae:6e:f9:71:d3:54:6b:56
vagrant@vagrant:/etc/vault$

vault write pki_int/intermediate/generate/internal common_name="olegrovenskiy.com Intermediate Authority"

vagrant@vagrant:/etc/vault$
vagrant@vagrant:/etc/vault$ vault write pki_int/intermediate/generate/internal common_name="olegrovenskiy.com Intermediate Authority"
Key    Value
---    -----
csr    -----BEGIN CERTIFICATE REQUEST-----
MIICeDCCAWACAQAwMzExMC8GA1UEAxMob2xlZ3JvdmVuc2tpeS5jb20gSW50ZXJt
ZWRpYXRlIEF1dGhvcml0eTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB
AKTMyR0lYUtcb5JbDN04taItrfM64qb/q6uyO+0va7LPSiLCZ9ohq5GULmrZws3l
Tf1CBnJtYRsjQfyGv3SpmvBFKJl4kjvbze2Jxq0VQyi/G5xioHgQw7H32G17qigB
krEh/ODwdjxO8Tbu8AxscBfD5fFilVgTV5N6SRPrSZXsRGI71xDOY63Uha9YOMSM
yguPuUA/VRuzrmLq7Zrj9n6Jbo+VIVasw+YKIBMp0SQU16IiCl3z6vCP0A8P9y2O
+HYQrzT2rSvcZI8K6sYit2hsw0tlBwQcmteR2dOGk8W5UHKhPiDHywSNu6D4mv2R
Y69JoD3ysJsuq/t1hwf5vT8CAwEAAaAAMA0GCSqGSIb3DQEBCwUAA4IBAQCUMU3m
t4966Tya5pi8g0Ys5WeJI9rkrY4/bCIpvwNZIs7Z3myuXn0JswuIpjm95z1p/tQ+
FFb4Ix7V4OGzlL0v5aB7PIzTsArU6vTWBnM+Cmomdrp9UUbee20B0IebnEyAm9QQ
E5o++n+cGCEtcA5js/cND+pywdnlBcxochzBPHZWXlOoRHc8QYVTJ7bpSwqT7aHa
xlTxuT4jW4UkuyJCAsIhifPMAbN38rusca2y36CDVpQIYjJB0Q2n22m6yWJ6fSfo
Cc5n1ThVwL2jh+Inyng3WpkBlC0+JHQAE9chG++oFEuLhE/4U0oOMFni9AgAJ4Tg
fUHoDm7IkCF+V+oR
-----END CERTIFICATE REQUEST-----
vagrant@vagrant:/etc/vault$

-----------------------------

С Вагрант не удобно, поэтому проелал все пункты перечисленные выше
на рабочем сервере, где усть полные права root

-------------------------------------------

4. Согласно этой же инструкции, подпишите Intermediate CA csr на сертификат для тестового домена (например, 
netology.example.com если действовали согласно инструкции).

vault write pki_int/issue/example-dot-com common_name="netology.example.com" ttl="240h"


[root@mck-test-network-1 etc]#
[root@mck-test-network-1 etc]# vault write pki_int/issue/example-dot-com common_name="netology.example.com" ttl="240h"
Key                 Value
---                 -----
ca_chain            
-----BEGIN CERTIFICATE-----
MIIDqjCCApKgAwIBAgIUH0wavwHI7c3De4m6baaHu2Roou4wDQYJKoZIhvcNAQEL
BQAwFjEUMBIGA1UEAxMLZXhhbXBsZS5jb20wHhcNMjEwNzAyMDcwNTEzWhcNMjYw
NzAxMDcwNTQzWjAtMSswKQYDVQQDEyJleGFtcGxlLmNvbSBJbnRlcm1lZGlhdGUg
QXV0aG9yaXR5MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArFtCnzx4
0F+Qp/1mV9zS9YTVOoD5IoPtCNeeE0ZZSBI5Kgn5J+sG6U48Yyqh8E+5zIM6YOLR
YQoKnFOJm3jSKRmWDwVqv565d9pwHMdIFrwDKfe96nzgx9q1lLqHxHFyZWRKsQNc
NePwVT4ojIkK13WFwWSIzphGHQKkuU6ZoRWnxDg/a3aAmU+brBvDLmindiJ6QzA+
tRa3OUcnVbvAnwTSnlyePHhO2f/m3jhLcR/gMpx9+UhipGni5oBDLoQ44qsxmV/j
GW3MEse/eg8cHsxpzLSiRCklu2dqYus8mzku5N6Zz51SSOqebbkDK/qV1VH/Bm6O
rxMell3kJvO4uQIDAQABo4HYMIHVMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMBAf8E
BTADAQH/MB0GA1UdDgQWBBSIcZhY3cuddYmRpqAXPbiWqLzPxjAfBgNVHSMEGDAW
gBRvUSCIxQbjeOzZXGtOAg1UR21NeTA9BggrBgEFBQcBAQQxMC8wLQYIKwYBBQUH
MAKGIWh0dHA6Ly8xMC4xMDIuMS4zMTo4MjAwL3YxL3BraS9jYTAzBgNVHR8ELDAq
MCigJqAkhiJodHRwOi8vMTAuMTAyLjEuMzE6ODIwMC92MS9wa2kvY3JsMA0GCSqG
SIb3DQEBCwUAA4IBAQDNjOUTnR/RU4OvRd7Foxzd0z44iMOIJUUiiCC/Rmn8xDG2
p1b2MyhT2BwNuO65mGuptXRMSrSdCxDEBCdz7/6/tkio190zcj3+izUZjao/L/3d
sCPwke/uUCNLV/ct1ggMl898U3wJqtZ8y8B6M75vBy7vZQvsGZc44MrnWXBqKMgu
+6vT3QjSQyJxKRZ5+MbEgz8FEINRBiQ/VcJ2LGLYfP61OxNC/IAbhI/QPMrp+4FT
0gwm7t/m4WLG5ktbuLbBOGxPiLDR0MnE1hMxYqPBO0zPD3G4mIwlvh4AYOHPKD+j
HvU6LXSIGJ46GJM9T0Zo4xZnXzxzEoCO+kEuaRcv
-----END CERTIFICATE-----
certificate

         
-----BEGIN CERTIFICATE-----
MIIDbjCCAlagAwIBAgIUeEd29tB0S/ASJhWAfE9OCIb6cGcwDQYJKoZIhvcNAQEL
BQAwLTErMCkGA1UEAxMiZXhhbXBsZS5jb20gSW50ZXJtZWRpYXRlIEF1dGhvcml0
eTAeFw0yMTA3MDIwNzEzMTFaFw0yMTA3MTIwNzEzNDFaMB8xHTAbBgNVBAMTFG5l
dG9sb2d5LmV4YW1wbGUuY29tMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKC
AQEAn+B9Vty2PIm45LmHmle/HNXSJsW+tjbRv4Lt7KmR+Y+1ewOrl9xb3As6FBfK
/czlnXmVk0L0ZN49RAdRF9/xU6c/tJnS/bS8xMlgPlQ3MgAgCzI/wwfSNcz3lNmv
qtMFWNfNEOTsvBMuUOKYKw2TjgYQO9KjdRhcgydEQsEPmrx1h0OOUC966Sf+msiQ
4VY2twec5XFoz7F15p7YFg9Ou3fEcjMtJ2ZtM1jVUhB4oR/HcK25QQRa8SyvKUF5
D29loHxpEPFVwTQR/mJ9eu7jlXvIF4LFCel+Tef5DqfKOuuWuToqW90MayMDLTSf
li19zHxcBumaWNrViObsEPfM8wIDAQABo4GTMIGQMA4GA1UdDwEB/wQEAwIDqDAd
BgNVHSUEFjAUBggrBgEFBQcDAQYIKwYBBQUHAwIwHQYDVR0OBBYEFFHMKYs6KQbO
xYEO1woCgk06ucglMB8GA1UdIwQYMBaAFIhxmFjdy511iZGmoBc9uJaovM/GMB8G
A1UdEQQYMBaCFG5ldG9sb2d5LmV4YW1wbGUuY29tMA0GCSqGSIb3DQEBCwUAA4IB
AQBW7ZCU7jVoy4RLPz9qBJEVM/PMM76/IzmcIrdEceAIpvPr6v1RbcRV9t1vbs8X
OQs8D1J9OuW0fxtIRBJH5hZ0tHgo5Wqm+7+jtwJuKbT09j8BRcwnOG9YQVWi4e9D
dVz1PHip5RCRxgAyXiVnVKybUXfboO6dobJoceVJ4VfZwBi0gd0C3K8i6Bbhdevc
Nd3den3aS7AQ9bGIQn8r/+zSM8yTLMH+SUxKQNq7iEtYkyxv62C1h37z1KGbCD07
urKqB+7QrbDj535Mr97Zno7ge9RexG/2unZLMTrN8E0nwwL7jd4Hs2KIUSmzNhYy
vdAqtl7egGVce0THQjtHvjXv
-----END CERTIFICATE-----
expiration          1626074021
issuing_ca          

-----BEGIN CERTIFICATE-----
MIIDqjCCApKgAwIBAgIUH0wavwHI7c3De4m6baaHu2Roou4wDQYJKoZIhvcNAQEL
BQAwFjEUMBIGA1UEAxMLZXhhbXBsZS5jb20wHhcNMjEwNzAyMDcwNTEzWhcNMjYw
NzAxMDcwNTQzWjAtMSswKQYDVQQDEyJleGFtcGxlLmNvbSBJbnRlcm1lZGlhdGUg
QXV0aG9yaXR5MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArFtCnzx4
0F+Qp/1mV9zS9YTVOoD5IoPtCNeeE0ZZSBI5Kgn5J+sG6U48Yyqh8E+5zIM6YOLR
YQoKnFOJm3jSKRmWDwVqv565d9pwHMdIFrwDKfe96nzgx9q1lLqHxHFyZWRKsQNc
NePwVT4ojIkK13WFwWSIzphGHQKkuU6ZoRWnxDg/a3aAmU+brBvDLmindiJ6QzA+
tRa3OUcnVbvAnwTSnlyePHhO2f/m3jhLcR/gMpx9+UhipGni5oBDLoQ44qsxmV/j
GW3MEse/eg8cHsxpzLSiRCklu2dqYus8mzku5N6Zz51SSOqebbkDK/qV1VH/Bm6O
rxMell3kJvO4uQIDAQABo4HYMIHVMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMBAf8E
BTADAQH/MB0GA1UdDgQWBBSIcZhY3cuddYmRpqAXPbiWqLzPxjAfBgNVHSMEGDAW
gBRvUSCIxQbjeOzZXGtOAg1UR21NeTA9BggrBgEFBQcBAQQxMC8wLQYIKwYBBQUH
MAKGIWh0dHA6Ly8xMC4xMDIuMS4zMTo4MjAwL3YxL3BraS9jYTAzBgNVHR8ELDAq
MCigJqAkhiJodHRwOi8vMTAuMTAyLjEuMzE6ODIwMC92MS9wa2kvY3JsMA0GCSqG
SIb3DQEBCwUAA4IBAQDNjOUTnR/RU4OvRd7Foxzd0z44iMOIJUUiiCC/Rmn8xDG2
p1b2MyhT2BwNuO65mGuptXRMSrSdCxDEBCdz7/6/tkio190zcj3+izUZjao/L/3d
sCPwke/uUCNLV/ct1ggMl898U3wJqtZ8y8B6M75vBy7vZQvsGZc44MrnWXBqKMgu
+6vT3QjSQyJxKRZ5+MbEgz8FEINRBiQ/VcJ2LGLYfP61OxNC/IAbhI/QPMrp+4FT
0gwm7t/m4WLG5ktbuLbBOGxPiLDR0MnE1hMxYqPBO0zPD3G4mIwlvh4AYOHPKD+j
HvU6LXSIGJ46GJM9T0Zo4xZnXzxzEoCO+kEuaRcv
-----END CERTIFICATE-----



private_key         
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAn+B9Vty2PIm45LmHmle/HNXSJsW+tjbRv4Lt7KmR+Y+1ewOr
l9xb3As6FBfK/czlnXmVk0L0ZN49RAdRF9/xU6c/tJnS/bS8xMlgPlQ3MgAgCzI/
wwfSNcz3lNmvqtMFWNfNEOTsvBMuUOKYKw2TjgYQO9KjdRhcgydEQsEPmrx1h0OO
UC966Sf+msiQ4VY2twec5XFoz7F15p7YFg9Ou3fEcjMtJ2ZtM1jVUhB4oR/HcK25
QQRa8SyvKUF5D29loHxpEPFVwTQR/mJ9eu7jlXvIF4LFCel+Tef5DqfKOuuWuToq
W90MayMDLTSfli19zHxcBumaWNrViObsEPfM8wIDAQABAoIBAGNPKew720NNdTk2
eaII4WDC/PAyox1CfhWM+cepKVCw0NUh2YPSUIklvCThBqmSjgq8jInV7EN/vOS1
+sxuwdPpruu7JVGM5DkEsbDl1QdNBpqN0weNoyjiMeQXOERPIiImom3dFaRZ8coy
hr0viLmO0KSoWfqRcF3TlVY14ECH1oo5kR3mI9kQLX/ONxRNnTPK4NTmJlo/VHCF
04jLrL3MiHgJ+sFb6eu28JrOBjQuE+gO13QGpuZ+fchZ+wHdnjfOAVMlFgv7kTF5
EsIo8bN4Z8UxOtFN0LQrEE2l0TjSnAxNTxwdBL/OfZZSGedsXi6IgtJaZ6FJ4Yjw
B7msTTkCgYEAwSAe4RTYmKHb9bQG0bK8dl2o95ZPTedaU4anM30pkJsw9V3TyxpM
FP2TArKufPehN/WNFH9a51mRyN0IT+hyPJ9xyI9iTkb2JiEGjETVWdtC4wC1Mkge
a836iYMqnCychkY0/SbPUyob5nFHAylErvUyVlDO/LufFKAmcio+FFcCgYEA0+1L
wf5dN9H3ITTpnE10Pky0P2YEbyiskOWVoSWwrTgyS0StpSdtxdcmaAb4R2zSINbx
FIYCGfrKq/9qpjaf41l+oXrYpEARri4gnvsgs5ZtXwa8XiZpgW35pSTVjBczr033
Rr4JNZeQFJKghLoX2uC67Kiesz+pv1gv/JKXSsUCgYEAuzGLPNib6bZaIprRUUlS
a9j1AqdrTzPE1dlbEAlt1IDYv7ymoeNng6EWcjMH9pGAb2FP0mJvlne3W18Dw5Cn
yiMygxiYTQ9zYBn64tOFiYeCGc6B068b7ZrGEaxWDPMg9PXwPsDzjMTwLjn2fxXt
QTjiBdBmEYs68x8YpOhVLBkCgYAafY76sND2KUi63eJVp1jgcLYXNqlXO75WXlxV
yGBNRrkCr5MFEeim0j36wuRGCVQ6xqNb7WRV2wN6fHLYU/uob4dkp/ZskZWkMB/j
v4BW8na5ah4hpquJgjWybuhCmqPbReOi9B4ylL9t0uY9sQVKVs0GyA0OWubdBCj7
aVeAAQKBgCPOjFdJ0SxorJ/OB/oph9Bh6z7WXPxGIzy5ezv0ty9Mj96EOxWwpn/Z
zaTxG3orvHBE2CSskIC3RJKPqduTQqmSVL7WupadMY0RISbY5ZS9fT5nHXqaxWto
hFlyuNIhvN66DUk1MJC3P3TH/iR2A/Jxkp0eNDWv1w6J4je6vnre
-----END RSA PRIVATE KEY-----
private_key_type    rsa
serial_number       78:47:76:f6:d0:74:4b:f0:12:26:15:80:7c:4f:4e:08:86:fa:70:67
[root@mck-test-network-1 etc]#


5. Поднимите на localhost nginx, сконфигурируйте default vhost для использования подписанного 
Vault Intermediate CA сертификата и выбранного вами домена. 
Сертификат из Vault подложить в nginx руками.

на хосте поднял nginx и сделал default хост 

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-available/*.conf;



и хост netology


[root@mck-test-network-1 nginx]# nginx -T | grep netology.example.com
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
server_name netology.example.com;
[root@mck-test-network-1 nginx]#


проверяю с другого сервера

[root@mck-test-network-2 keepalived]#
[root@mck-test-network-2 keepalived]# curl 10.102.1.31 -H "Host: netology.example.com"
<html>
  <head>
    <title>mysite.ru</title>
  </head>
  <body>
    <h1>Ура! Вы смогли настроить Virtual Host в nginx!</h1>
  </body>
</html>
[root@mck-test-network-2 keepalived]#

Проверка сертификата:

[root@mck-test-network-1 certs]# cd /usr/share/nginx/html/example
[root@mck-test-network-1 example]# curl -k https://netology.example.com
curl: (7) Failed connect to netology.example.com:443; Connection refused
[root@mck-test-network-1 example]# curl --cacert cert.pem https://netology.example.com
curl: (7) Failed connect to netology.example.com:443; Connection refused
[root@mck-test-network-1 example]#

ошибка, порт 443 не открыт и сертификат ещё не выложен.

После загрузки сертификата и ключа:


[root@mck-test-network-1 certs]# cat chain.pem

-----BEGIN CERTIFICATE-----
MIIDbjCCAlagAwIBAgIUeEd29tB0S/ASJhWAfE9OCIb6cGcwDQYJKoZIhvcNAQEL
BQAwLTErMCkGA1UEAxMiZXhhbXBsZS5jb20gSW50ZXJtZWRpYXRlIEF1dGhvcml0
eTAeFw0yMTA3MDIwNzEzMTFaFw0yMTA3MTIwNzEzNDFaMB8xHTAbBgNVBAMTFG5l
dG9sb2d5LmV4YW1wbGUuY29tMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKC
AQEAn+B9Vty2PIm45LmHmle/HNXSJsW+tjbRv4Lt7KmR+Y+1ewOrl9xb3As6FBfK
/czlnXmVk0L0ZN49RAdRF9/xU6c/tJnS/bS8xMlgPlQ3MgAgCzI/wwfSNcz3lNmv
qtMFWNfNEOTsvBMuUOKYKw2TjgYQO9KjdRhcgydEQsEPmrx1h0OOUC966Sf+msiQ
4VY2twec5XFoz7F15p7YFg9Ou3fEcjMtJ2ZtM1jVUhB4oR/HcK25QQRa8SyvKUF5
D29loHxpEPFVwTQR/mJ9eu7jlXvIF4LFCel+Tef5DqfKOuuWuToqW90MayMDLTSf
li19zHxcBumaWNrViObsEPfM8wIDAQABo4GTMIGQMA4GA1UdDwEB/wQEAwIDqDAd
BgNVHSUEFjAUBggrBgEFBQcDAQYIKwYBBQUHAwIwHQYDVR0OBBYEFFHMKYs6KQbO
xYEO1woCgk06ucglMB8GA1UdIwQYMBaAFIhxmFjdy511iZGmoBc9uJaovM/GMB8G
A1UdEQQYMBaCFG5ldG9sb2d5LmV4YW1wbGUuY29tMA0GCSqGSIb3DQEBCwUAA4IB
AQBW7ZCU7jVoy4RLPz9qBJEVM/PMM76/IzmcIrdEceAIpvPr6v1RbcRV9t1vbs8X
OQs8D1J9OuW0fxtIRBJH5hZ0tHgo5Wqm+7+jtwJuKbT09j8BRcwnOG9YQVWi4e9D
dVz1PHip5RCRxgAyXiVnVKybUXfboO6dobJoceVJ4VfZwBi0gd0C3K8i6Bbhdevc
Nd3den3aS7AQ9bGIQn8r/+zSM8yTLMH+SUxKQNq7iEtYkyxv62C1h37z1KGbCD07
urKqB+7QrbDj535Mr97Zno7ge9RexG/2unZLMTrN8E0nwwL7jd4Hs2KIUSmzNhYy
vdAqtl7egGVce0THQjtHvjXv
-----END CERTIFICATE-----
 -----BEGIN CERTIFICATE-----
MIIDqjCCApKgAwIBAgIUH0wavwHI7c3De4m6baaHu2Roou4wDQYJKoZIhvcNAQEL
BQAwFjEUMBIGA1UEAxMLZXhhbXBsZS5jb20wHhcNMjEwNzAyMDcwNTEzWhcNMjYw
NzAxMDcwNTQzWjAtMSswKQYDVQQDEyJleGFtcGxlLmNvbSBJbnRlcm1lZGlhdGUg
QXV0aG9yaXR5MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArFtCnzx4
0F+Qp/1mV9zS9YTVOoD5IoPtCNeeE0ZZSBI5Kgn5J+sG6U48Yyqh8E+5zIM6YOLR
YQoKnFOJm3jSKRmWDwVqv565d9pwHMdIFrwDKfe96nzgx9q1lLqHxHFyZWRKsQNc
NePwVT4ojIkK13WFwWSIzphGHQKkuU6ZoRWnxDg/a3aAmU+brBvDLmindiJ6QzA+
tRa3OUcnVbvAnwTSnlyePHhO2f/m3jhLcR/gMpx9+UhipGni5oBDLoQ44qsxmV/j
GW3MEse/eg8cHsxpzLSiRCklu2dqYus8mzku5N6Zz51SSOqebbkDK/qV1VH/Bm6O
rxMell3kJvO4uQIDAQABo4HYMIHVMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMBAf8E
BTADAQH/MB0GA1UdDgQWBBSIcZhY3cuddYmRpqAXPbiWqLzPxjAfBgNVHSMEGDAW
gBRvUSCIxQbjeOzZXGtOAg1UR21NeTA9BggrBgEFBQcBAQQxMC8wLQYIKwYBBQUH
MAKGIWh0dHA6Ly8xMC4xMDIuMS4zMTo4MjAwL3YxL3BraS9jYTAzBgNVHR8ELDAq
MCigJqAkhiJodHRwOi8vMTAuMTAyLjEuMzE6ODIwMC92MS9wa2kvY3JsMA0GCSqG
SIb3DQEBCwUAA4IBAQDNjOUTnR/RU4OvRd7Foxzd0z44iMOIJUUiiCC/Rmn8xDG2
p1b2MyhT2BwNuO65mGuptXRMSrSdCxDEBCdz7/6/tkio190zcj3+izUZjao/L/3d
sCPwke/uUCNLV/ct1ggMl898U3wJqtZ8y8B6M75vBy7vZQvsGZc44MrnWXBqKMgu
+6vT3QjSQyJxKRZ5+MbEgz8FEINRBiQ/VcJ2LGLYfP61OxNC/IAbhI/QPMrp+4FT
0gwm7t/m4WLG5ktbuLbBOGxPiLDR0MnE1hMxYqPBO0zPD3G4mIwlvh4AYOHPKD+j
HvU6LXSIGJ46GJM9T0Zo4xZnXzxzEoCO+kEuaRcv
-----END CERTIFICATE-----
[root@mck-test-network-1 certs]#
[root@mck-test-network-1 certs]#
[root@mck-test-network-1 certs]#
[root@mck-test-network-1 certs]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
[root@mck-test-network-1 certs]#
[root@mck-test-network-1 certs]#
[root@mck-test-network-1 certs]# cat /etc/nginx/sites-available/example.conf
server {
listen 443 ssl;
server_name netology.example.com;
root /usr/share/nginx/html/example;
index index.html index.htm;
ssl_certificate /etc/ssl/certs/chain.pem;
ssl_certificate_key /etc/ssl/certs/key.pem;

location / {}
}

[root@mck-test-network-1 certs]# cat key.pem
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAn+B9Vty2PIm45LmHmle/HNXSJsW+tjbRv4Lt7KmR+Y+1ewOr
l9xb3As6FBfK/czlnXmVk0L0ZN49RAdRF9/xU6c/tJnS/bS8xMlgPlQ3MgAgCzI/
wwfSNcz3lNmvqtMFWNfNEOTsvBMuUOKYKw2TjgYQO9KjdRhcgydEQsEPmrx1h0OO
UC966Sf+msiQ4VY2twec5XFoz7F15p7YFg9Ou3fEcjMtJ2ZtM1jVUhB4oR/HcK25
QQRa8SyvKUF5D29loHxpEPFVwTQR/mJ9eu7jlXvIF4LFCel+Tef5DqfKOuuWuToq
W90MayMDLTSfli19zHxcBumaWNrViObsEPfM8wIDAQABAoIBAGNPKew720NNdTk2
eaII4WDC/PAyox1CfhWM+cepKVCw0NUh2YPSUIklvCThBqmSjgq8jInV7EN/vOS1
+sxuwdPpruu7JVGM5DkEsbDl1QdNBpqN0weNoyjiMeQXOERPIiImom3dFaRZ8coy
hr0viLmO0KSoWfqRcF3TlVY14ECH1oo5kR3mI9kQLX/ONxRNnTPK4NTmJlo/VHCF
04jLrL3MiHgJ+sFb6eu28JrOBjQuE+gO13QGpuZ+fchZ+wHdnjfOAVMlFgv7kTF5
EsIo8bN4Z8UxOtFN0LQrEE2l0TjSnAxNTxwdBL/OfZZSGedsXi6IgtJaZ6FJ4Yjw
B7msTTkCgYEAwSAe4RTYmKHb9bQG0bK8dl2o95ZPTedaU4anM30pkJsw9V3TyxpM
FP2TArKufPehN/WNFH9a51mRyN0IT+hyPJ9xyI9iTkb2JiEGjETVWdtC4wC1Mkge
a836iYMqnCychkY0/SbPUyob5nFHAylErvUyVlDO/LufFKAmcio+FFcCgYEA0+1L
wf5dN9H3ITTpnE10Pky0P2YEbyiskOWVoSWwrTgyS0StpSdtxdcmaAb4R2zSINbx
FIYCGfrKq/9qpjaf41l+oXrYpEARri4gnvsgs5ZtXwa8XiZpgW35pSTVjBczr033
Rr4JNZeQFJKghLoX2uC67Kiesz+pv1gv/JKXSsUCgYEAuzGLPNib6bZaIprRUUlS
a9j1AqdrTzPE1dlbEAlt1IDYv7ymoeNng6EWcjMH9pGAb2FP0mJvlne3W18Dw5Cn
yiMygxiYTQ9zYBn64tOFiYeCGc6B068b7ZrGEaxWDPMg9PXwPsDzjMTwLjn2fxXt
QTjiBdBmEYs68x8YpOhVLBkCgYAafY76sND2KUi63eJVp1jgcLYXNqlXO75WXlxV
yGBNRrkCr5MFEeim0j36wuRGCVQ6xqNb7WRV2wN6fHLYU/uob4dkp/ZskZWkMB/j
v4BW8na5ah4hpquJgjWybuhCmqPbReOi9B4ylL9t0uY9sQVKVs0GyA0OWubdBCj7
aVeAAQKBgCPOjFdJ0SxorJ/OB/oph9Bh6z7WXPxGIzy5ezv0ty9Mj96EOxWwpn/Z
zaTxG3orvHBE2CSskIC3RJKPqduTQqmSVL7WupadMY0RISbY5ZS9fT5nHXqaxWto
hFlyuNIhvN66DUk1MJC3P3TH/iR2A/Jxkp0eNDWv1w6J4je6vnre
-----END RSA PRIVATE KEY-----
[root@mck-test-network-1 certs]#



тест конфигурации и рестарт nginx

[root@mck-test-network-1 certs]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
[root@mck-test-network-1 certs]# systemctl restart nginx
[root@mck-test-network-1 certs]#


Но curl выдаёт ошибку

[root@mck-test-network-1 certs]# curl -k https://netology.example.com
<html>
  <head>
    <title>netology.example.com</title>
  </head>
  <body>
    <h1>Ура! Вы смогли настроить Virtual Host в nginx!</h1>
  </body>
</html>
[root@mck-test-network-1 certs]# curl --cacert cert.pem https://netology.example.com
curl: (77) Problem with the SSL CA cert (path? access rights?)
[root@mck-test-network-1 certs]#


[root@mck-test-network-1 certs]# curl -v https://netology.example.com
* About to connect() to netology.example.com port 443 (#0)
*   Trying 127.0.0.1...
* Connected to netology.example.com (127.0.0.1) port 443 (#0)
* Initializing NSS with certpath: sql:/etc/pki/nssdb
*   CAfile: /etc/pki/tls/certs/ca-bundle.crt
  CApath: none
* Server certificate:
*       subject: CN=netology.example.com
*       start date: Jul 02 07:13:11 2021 GMT
*       expire date: Jul 12 07:13:41 2021 GMT
*       common name: netology.example.com
*       issuer: CN=example.com Intermediate Authority
* NSS error -8179 (SEC_ERROR_UNKNOWN_ISSUER)
* Peer's Certificate issuer is not recognized.
* Closing connection 0
curl: (60) Peer's Certificate issuer is not recognized.
More details here: http://curl.haxx.se/docs/sslcerts.html

curl performs SSL certificate verification by default, using a "bundle"
 of Certificate Authority (CA) public keys (CA certs). If the default
 bundle file isn't adequate, you can specify an alternate file
 using the --cacert option.
If this HTTPS server uses a certificate signed by a CA represented in
 the bundle, the certificate verification probably failed due to a
 problem with the certificate (it might be expired, or the name might
 not match the domain name in the URL).
If you'd like to turn off curl's verification of the certificate, use
 the -k (or --insecure) option.
[root@mck-test-network-1 certs]#


------------------------------------------

Итоговый результат

После корректировки файлов ca-bandle.cert и ca-bundle.trusted.cert, добавив в них 
интермедиате сертификаты

[root@mck-test-network-1 certs]# curl -v https://netology.example.com
* About to connect() to netology.example.com port 443 (#0)
*   Trying 127.0.0.1...
* Connected to netology.example.com (127.0.0.1) port 443 (#0)
* Initializing NSS with certpath: sql:/etc/pki/nssdb
*   CAfile: /etc/pki/tls/certs/ca-bundle.crt
  CApath: none
* SSL connection using TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
* Server certificate:
*       subject: CN=netology.example.com
*       start date: Jul 02 07:13:11 2021 GMT
*       expire date: Jul 12 07:13:41 2021 GMT
*       common name: netology.example.com
*       issuer: CN=example.com Intermediate Authority
> GET / HTTP/1.1
> User-Agent: curl/7.29.0
> Host: netology.example.com
> Accept: */*
>
< HTTP/1.1 200 OK
< Server: nginx/1.20.1
< Date: Mon, 05 Jul 2021 08:00:30 GMT
< Content-Type: text/html
< Content-Length: 174
< Last-Modified: Mon, 05 Jul 2021 05:36:52 GMT
< Connection: keep-alive
< ETag: "60e29a74-ae"
< Accept-Ranges: bytes
<
<html>
  <head>
    <title>netology.example.com</title>
  </head>
  <body>
    <h1>Ура! Вы смогли настроить Virtual Host в nginx!</h1>
  </body>
</html>
* Connection #0 to host netology.example.com left intact
[root@mck-test-network-1 certs]#


судя по выводу с сертификатом теперь всё ок

7. Ознакомился с ACME и CA Let's encrypt.
















