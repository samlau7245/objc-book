# simctl

> `simctl` 虚拟机操作

# altool

>* `altool` 可对您的 App 进行公证以便您自行分发；也可用于验证和上传您 App 的二进制文件，以通过 App Store 进行分发。
>* `altool`位置:`/Applications/Xcode.app/Contents/Developer/usr/bin/altool`。
>* 在2019年6月1日之后签名或构建的所有新 App 均需进行公证。

```sh
# 验证APP
xcrun altool --validate-app -f file [-t platform] -u username [-p password] [--output-format xml]
# 上传APP到 App Store
xcrun altool --upload-app -f file [-t platform] -u username [-p password] [--output-format xml]
# 公证APP。 如果成功运行，系统将为您返回与上传文件相关联的 UUID（通用唯一识别码），您可以使用该 UUID 检索有关上传文件的信息。
xcrun altool --notarize-app -f file --primary-bundle-id bundle_id -u username -p password
```

* `--validate-app` 验证指定的 App。验证流程用于检查 App 是否满足 App Store 的最低要求，并确保其能够通过 App Store Connect 的基本检查。
* `--upload-app` 将指定的 App 上传到 App Store。
* `-f file` 待验证或待上传的 App 的路径和文件名。
* `-t platform` 可选。表示文件适用的平台。可指定为 `osx`、`ios` 或 `appletvos`。
* `-u username` 您的用户名。请注意，您指定的 username 应是您的 App Store Connect 用户名。
	* 注意：如果您使用了 username 选项，则必须指定 password。您也可以使用 apiKey／apiIssuer 进行身份验证。
* `-p password` 您的用户密码。如果您指定了 username 而没有指定 apiKey／apiIssuer，则需要提供密码。如果您未在命令行中指定 password，则此项将从 stdin（标准输入）读取。
	* 除了用纯文本格式输入 password，您还可以使用 `@keychain:` 或 `@env:` 前缀，后跟钥匙串密码项目名称或环境变量名称以进行指定。
	* 钥匙串示例：`-p @keychain:<SECRET>` 使用存储在钥匙串密码项目 `<SECRET>` 中的密码，其帐户值与指定的用户名匹配。
	* 环境示例：`-p @env:<SECRET>` 使用环境变量 `<SECRET>` 中的值。
* `--store-password-in-keychain-item <name_for_keychain_item>` 以指定的名称创建钥匙串项目，将其与帐户 username 关联，并将密码 password 存储在该钥匙串项目中。
	* 您可以将此钥匙串项目与 -p 选项一起使用，以便用其他命令遮盖您的密码。
	* 如果钥匙串中已存在具有指定名称和帐户的钥匙串项目，则项目密码将会更新。如果不存在此项目，将使用指定的名称创建新项目。
	* **创建／存储示例** `altool --store-password-in-keychain-item MY_SECRET -u jappleseed@apple.com -p "MyP@ssw0rd!@78"` 将密码存储在名为 `<MY_SECRET>` 的钥匙串项目中。
	* **钥匙串使用示例** `altool --notarize-app -u jappleseed@apple.com -p @keychain:MY_SECRET [...]`
		* 使用存储在钥匙串密码项目 `<MY_SECRET>` 中的密码，该密码项目的帐户值与指定的用户名一致。
* `--apiKey api_key` 提供 API 密钥（密钥 ID）; 如果您未使用 username／password 验证，则需要提供 apiKey／apiIssuer。如果您指定了 --apiKey，则必须指定 --apiIssuer。
* `--apiIssuer issuer_id` API 发放者信息（Issuer ID）用于身份验证。
* `--output-format [xml | json | normal]` 指定 altool 返回输出信息的格式：结构化的 XML 或 json 格式，或非结构化的文本格式（normal）。altool 会默认以文本格式返回输出信息。
* `--notarize-app` 上传指定的 App 文件（.pkg、.dmg 或 .zip 格式）以进行公证。
* `--primary-bundle-id bundle_id` 待公证 App 的唯一标识符。使用 `--notarize-app` 时需要提供。
* `--list-providers` 列出与您帐户关联的提供商，以及提供商的简称、团队 ID 和 public ID（公开 ID）。借助此命令，您可以确定用于 --asc-provider、--team-id 和 --asc-public-id 选项的信息。使用此命令需要验证身份。
* `--asc-provider <provider_shortname>` 提供商的简称。如果您的用户帐户与多个提供商相关联，则使用 --notarize-app 和 --notarization-history 时需要提供此信息。
	* **注意**：如果用户帐户与多个提供商相关联，您至少应提供以下信息之一：--asc-provider、--team-id 或 --asc-public-id。
* `--asc-public-id  <public_id>` 如果您的帐户与多个提供商相关联，则使用 --notarize-app 和 --notarization-history 时需要提供此信息。您可以使用 —list-providers 命令来检索与您帐户关联的提供商。
	* **注意**：如果用户帐户与多个提供商相关联，您至少应提供以下信息之一：--asc-provider、--team-id 或 --asc-public-id。
* `--team-id  <wwdr_team_id>` 如果您的用户帐户与多个提供商相关联，则使用 --notarize-app 和 --notarization-history 时需要提供此信息。您可以使用 --list-providers 命令来检索与您帐户关联的提供商。
	* **注意**：如果用户帐户与多个提供商相关联，您至少应提供以下信息之一：--asc-provider、--team-id 或 --asc-public-id。

## 检索状态和日志文件

```sh
# 可以使用指定的 uuid 检索已上传 App 的公证状态和日志文件
# 根据指定的 UUID 返回相应 App 的状态和日志文件网址（URL）。使用此命令需要验证身份。
xcrun altool --notarization-info uuid -u username -p password
```

通过日志文件检查警告信息或查看公证凭证的具体内容: 

* `Processing（处理中）`：上传成功，正在处理 App。
* `Upload failed（上传失败）`：上传失败。
* `Ready for distribution（准备分发）`：处理已完成，您可以分发已公证的 App。
* `Rejected（已拒绝）`：存档文件无效或未通过安全检查。

## 检索公证历史

```sh
xcrun altool --notarization-history page -u username -p password
```

* `page` 参数用于指定条目范围，如果指定为 0 则将返回最新的多个条目。此外还会返回一个新的页面值，在下一次使用 --notarization-history 时可用作 page 值，依此类推，直到不再返回任何项目为止。
* 需要提供用户名和密码。如果您的帐户与多个提供商相关联，则还需要提供 --asc-provider。

## 发布内容

```sh
xcrun stapler staple <app bundle path>
```
## 资料

* [Apple-altool 指南](https://help.apple.com/asc/appsaltool/#/apdATD1E927-D1E1A1303-D1E927A1126)






































