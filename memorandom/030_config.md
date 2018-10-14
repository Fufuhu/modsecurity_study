

|名称|内容|備考|
|---|---|---|
|SecRuleEngine On/Off/DetectionOnly| ルールエンジンの有効化有無の確認|On/Off/DetectionOnly|
|SecRequestBodyAccess　On/Off|リクエストボディも含めて処理するか否かを定める|On/Off|
|SecRule VARIABLES OPERATOR [ACTIONS]|指定した変数、を指定したオペレータでルールを作成する||
|SecRequestBodyLimit LIMIT_IN_BYTES|ModSecurityがバッファリングするボディのサイズの最大値を指定する||
|SecRequestBodyNoFilesLimit LIMIT_IN_BYTES|ModSecurityがバッファリングするボディのサイズの最大値を指定する。ただし、転送される**ファイル**のサイズは除く|
|SecRequestBodyLimitAction Reject/ProcessPartial|ボディのリミットに抵触した場合に処理をどうするかを定める||
|SecPcreMatchLimit value|PCREライブラリにおけるマッチングのリミットを設定する|VirtualHostの中では定義できない<br/>libModSecurityではサポートされていない？|
|SecPcreMatchLimitRecursion value|PCREライブラリにおける再帰マッチングのリミットを設定する|libModSecurityではサポートされていない?|
|SecResponseBodyAccess On/Off|レスポンスボディへのアクセスを有効化するか否か||
|SecResponseBodyMimeType MIMETYPE MIMETYPE ...|レスポンスボディのバッファリング、分析対象となるMIMEタイプを指定する||
|SecResponseBodyLimit LIMIT_IN_BYTES|レスポンスボディでバッファリングして、分析対象とする上限サイズ||
|SecResponseBodyLimitAction Reject|ProcessPartial|レスポンスボディのバッファリング上限に抵触した場合の挙動を定める||
|SecTmpDir PATH|一時ファイルが作成されるディレクトリを指定する||
|SecDataDir PATH|永続化データの保管先のパス||
|SecAuditEngine On/Off/RelavantOnly|監査ログのエンジンを設定する|RelevantOnly:warning, errorや特定のステータスコードの場合のみ保管する|
|SecAuditLogRelevantStatus REGEX |監査ログに保管対象とするステータスコードを指定する||
|SecAuditLogParts PARTLETTERS|監査ログに何を保管するかを指定する|デフォルトは`ABCFHZ`|
|SecAuditLogType Serial/Concurrent/HTTPS|監査ログを保管するための機構を設定する||
|SecAuditLog|監査ログの保管先を指定する||
|SecArgumentSeparator SecArgumentSeparator character|application/x-www-form-urlencodedコンテンツのセパレータをして利用する文字列を指定する||
|SecCookieFormat 0/1|現在の設定フォーマットにおいて利用されるCookieのフォーマットを指定する|0:version 0フォーマット, 1: version 1フォーマット|
|SecUnicodeMapFile /path/to/unicode.mapping CODEPOINT|Unicodeのコードポイントマッピングを行うurlDecodeUni変換関数に用いられるファイルパス。
||
|SecStatusEngine|ステータスレポーティング機能の制御を行う。DNSベースのレポーティングを利用してModSecurityプロジェクトチームへのソフトウェアバージョンの通知を行う||

## SecAuditLogParts

A: Audit log header (mandatory).
B: Request headers.
C: Request body (present only if the request body exists and ModSecurity is configured to intercept it. This would require SecRequestBodyAccess to be set to on).
D: Reserved for intermediary response headers; not implemented yet.
E: Intermediary response body (present only if ModSecurity is configured to intercept response bodies, and if the audit log engine is configured to record it. Intercepting response bodies requires SecResponseBodyAccess to be enabled). Intermediary response body is the same as the actual response body unless ModSecurity intercepts the intermediary response body, in which case the actual response body will contain the error message (either the Apache default error message, or the ErrorDocument page).
F: Final response headers (excluding the Date and Server headers, which are always added by Apache in the late stage of content delivery).
G: Reserved for the actual response body; not implemented yet.
H: Audit log trailer.
I: This part is a replacement for part C. It will log the same data as C in all cases except when multipart/form-data encoding in used. In this case, it will log a fake application/x-www-form-urlencoded body that contains the information about parameters but not about the files. This is handy if you don’t want to have (often large) files stored in your audit logs.
J: This part contains information about the files uploaded using multipart/form-data encoding.
K: This part contains a full list of every rule that matched (one per line) in the order they were matched. The rules are fully qualified and will thus show inherited actions and default operators. Supported as of v2.5.0.
Z: Final boundary, signifies the end of the entry (mandatory).