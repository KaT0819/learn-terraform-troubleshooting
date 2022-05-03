# Learn Terraform Troubleshooting

This is a companion repository for the [Learn Terraform Troubleshooting](https://learn.hashicorp.com/tutorials/terraform/troubleshooting-workflow) tutorial on HashiCorp Learn. Follow along to learn more about configuration language troubleshooting.

## 手順

### 構文エラー
```
terraform fmt

terraform.tfvars
╷
│ Error: Invalid character
│ 
│   on main.tf line 49, in resource "aws_instance" "web_app":
│   49:     Name = $var.name-learn
│ 
│ This character is not used within the language.
╵

╷
│ Error: Invalid expression
│
│   on main.tf line 49, in resource "aws_instance" "web_app":
│   49:     Name = $var.name-learn
│
│ Expected the start of an expression, but found an invalid expression token.
```

```
# 変数の使い方
Name = $var.name-learn
↓
Name = "${var.name}-learn"
```


### 構成の依存関係エラー

``` terraform
terraform validate

 Error: Cycle: aws_security_group.sg_8080, aws_security_group.sg_ping

```

※チュートリアル通りにやるも謎の権限エラー
セキュリティグループがお互いに参照しあっているのがよくない？
ただ、セキュリティグループだけは作成されてしまっており、destroyは可能

```
creating EC2 Instance: UnauthorizedOperation: You are not authorized to perform this operation.
```


# コアロギングを有効
export TF_LOG_CORE=TRACE
# プロバイダーログを生成
export TF_LOG_PROVIDER=TRACE
