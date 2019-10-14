# タングル証明書作成者の概要
<!-- # Tangle certificate creator overview -->

**証明書は、所有者に関する事実を証明する公式文書です。しかし、証明書がデジタルの場合、それらは詐欺の対象となります。デジタル証明書はコピーおよび編集できるため、確実性と信用性に影響します。証明書が本物であり、信頼でき、変更されていないことを証明するために、証明書をタングルに添付することができます。**
<!-- **A certificate is an official document that proves a fact about the bearer. But, when certificates are digital, they are subject to fraud. Digital certificates can be copied and edited, which affects their authenticity and credibility. To prove that a certificate is authentic, credible, and unchanged, you can attach it to the Tangle.** -->

## イミュータブルな証明書を作成する
<!-- ## Create an immutable certificate -->

この例では、開発環境を使用して、[Tangle certificate API](https://certification-api.iota.org/docs/#iota-certification-api)を使用してTangleに証明書を添付します。
<!-- In this example, we use the [Tangle certificate API](https://certification-api.iota.org/docs/#iota-certification-api) to attach a certificate to the Tangle, using the development environment. -->

最初に、テンプレートの作成に使用できる証明書の背景を作成します。次に、テンプレートにアクセスし、テンプレートから証明書を作成できる発行者を作成します。
<!-- First, we create a certificate background that we can use to create a template. Then, we create an issuer who has access to the template and can create certificates from it. -->

:::info:
[タングル](root://dev-essentials/0.1/concepts/the-tangle.md)のトランザクションはイミュータブルであるため、トランザクションを真実の源として使用できます。
:::
<!-- :::info: -->
<!-- Transactions on [the Tangle](root://dev-essentials/0.1/concepts/the-tangle.md) are immutable, so we can use the transaction as a source of truth. -->
<!-- ::: -->

### 前提条件
<!-- ### Prerequisites -->

このチュートリアルを完了するには、次のものが必要です。
<!-- To complete this tutorial, you need the following: -->

* Node.js 8、またはNode.js 10以降。 [最新のLTS](https://nodejs.org/en/download/)をお勧めします。
<!-- * Node.js 8, or Node.js 10 or higher. We recommend the [latest LTS](https://nodejs.org/en/download/). -->
* [Visual Studio Code](https://code.visualstudio.com/Download)などのコードエディター
<!-- * A code editor such as [Visual Studio Code](https://code.visualstudio.com/Download) -->
* コマンドプロンプトへのアクセス
<!-- * Access to a command prompt -->
* インターネット接続
<!-- * An Internet connection -->
* [`axios`](https://www.npmjs.com/package/axios)パッケージ
<!-- * The [`axios`](https://www.npmjs.com/package/axios) package -->

### 手順1. API認証情報を取得する
<!-- ### Step 1. Get API credentials -->

タングル証明書APIを使用するには、IOTA財団のサーバーにアクセスする必要があります。
<!-- To use the Tangle certificate API, you need access to the IOTA Foundation server. -->

すべてのバックグラウンド、テンプレート、発行者、および証明書は、一意の組織IDでサーバーに保存されます。
<!-- All backgrounds, templates, issuers, and certificates are stored on the server under a unique organization ID. -->

組織の証明書データを作成、読み取り、更新、または削除するには、管理者IDと管理者パスフレーズが必要です。
<!-- To create, read, update, or delete any of an organization's certificate data, you need the administrator ID and the administrator passphrase. -->

独自の組織ID、管理者ID、および管理者パスフレーズを受け取るには、お問い合わせください。
<!-- Contact us to receive your own organization ID, administrator ID, and administrator passphrase -->

```bash
integrations@iota.org
```

### 手順2. バックグラウンドを作成する
<!-- ### Step 2. Create a background -->

証明書を作成する前に、バックグラウンドのSVGイメージが必要です。
<!-- Before you can create a certificate, you need a background SVG image. -->

このSVG画像には、二重のパーセント記号（%%）で囲まれたプレースホルダーを含めることができます。証明書を作成するときに、プレースホルダーを置き換えるコンテンツを指定できます。
<!-- This SVG image can include placeholders that are wrapped in double percentage signs (%%). When you come to create a certificate, you can specify the content to replace the placeholders. -->

作業ディレクトリに`background.svg`というファイルを作成し、次のコードをコピーします。
<!-- Create a file called `background.svg` in your working directory and copy in the following code -->

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg width="100%" height="100%" viewBox="0 0 842 596" version="1.1" xmlns="http://www.w3.org/2000/svg">
<rect x="0" y="0" width="842" height="596" style="fill:white;"/>
<rect x="30.81" y="30.82" width="779.66" height="533.38" style="fill:none;stroke:rgb(189,190,192);stroke-width:12px;"/>
<text x="100" y="100"> %%TEMPLATE-ADDITIONAL%%</text>
<text x="100" y="480" class="signature"> %%ISSUER-SIGNATURE%%</text>
<text x="50%" y="165" class="caption" text-anchor="middle">Certificate Of Attendance</text>
<text x="50%" y="220" class="training-title" text-anchor="middle">%%TRAINING-TITLE%%</text>
<text x="50%" y="295" class="participant" text-anchor="middle">%%PARTICIPANT%%</text>
<text x="50%" y="352" class="participation-date" text-anchor="middle">%%PARTICIPATION-DATE%%</text>
<text x="50%" y="390" class="info" text-anchor="middle">This certificate confirms that the training specified was attended and completed by the person on the date shown.</text>
<text x="50%" y="410" class="info" text-anchor="middle">You can validate the authenticity of this certificate by scanning the QR code, or by visiting %%AUTH-DOMAIN%%</text>
<text x="50%" y="420" class="info" text-anchor="middle">and entering the hash shown at the bottom of this certificate.</text>
<text x="17%" y="545" class="hash" text-anchor="start">Hash: %%HASH%%</text>
<text x="83%" y="545" class="hash" text-anchor="end">Issued On: %%ISSUED-DATE%%</text>
<svg x="44%" y="430">%%QR-CODE%%</svg>
<style>
.signature {
    font-family:"FreestyleScript-Regular", "Freestyle Script", cursive;
    font-size:33.333px;
}
.caption {
    font-family: "Nunito Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
    font-weight: 300;
    font-size: 26px;
}
.training-title {
    font-family: "Nunito Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
    font-weight: 600;
    font-size: 32px;
}
.participant {
    font-family: "Nunito Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
    font-weight: 600;
    font-size: 25px;
}
.participation-date {
    font-family: "Nunito Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
    font-weight: normal;
    font-size: 18px;
}
.info {
    font-family: "Nunito Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
    font-weight: 300;
    font-size: 10px;
}
.hash {
    font-family: "Nunito Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
    font-weight: normal;
    font-size: 8px;
}
</svg>
```

## 手順3. サンプルをセットアップする
<!-- ## Step 3. Set up the sample -->

1. `background.svg`ファイルと同じディレクトリに`index.js`という新しいファイルを作成し、次のコードをコピーします。 `$ORG_ID`、`$ADMIN_USER_ID`、および `$ADMIN_PASS_PHRASE`プレースホルダーを独自の認証情報に置き換えます。
<!-- 1. Create a new file called `index.js` in the same directory as the `background.svg` file, then copy in the following code. Replace the `$ORG_ID`, `$ADMIN_USER_ID`, and `$ADMIN_PASS_PHRASE` placeholders with your own credentials. -->

    ```js
    const fs = require('fs');
    const axios = require('axios');

    const API_ENDPOINT = 'https://certification-api.iota.works';
    const ORG_ID = '$ORG_ID'
    const ADMIN_USER_ID = '$ADMIN_USER_ID';
    const ADMIN_PASS_PHRASE = '$ADMIN_PASS_PHRASE';
    const BACKGROUND_PATH = 'background.svg';

    const ax = axios.create({ baseURL: API_ENDPOINT });

    async function createBackground() {
        console.log("Creating background...");

        // Call the background endpoint (https://certification-api.iota.org/docs/?javascript--nodejs#createupdatebackground) with your credentials
        const response = await ax.put(`background`, {
            adminId: ADMIN_USER_ID,
            adminPassPhrase: ADMIN_PASS_PHRASE,
            content: fs.readFileSync(BACKGROUND_PATH).toString()
        });

        if (response.data.success) {
            console.log("Created background", response.data.backgroundId);
            return response.data.backgroundId;
        } else {
            console.error("Failed creating background", response.data.message);
        }
    }

    async function createTemplate(backgroundId) {
        console.log("Creating template...");
        // // Call the template endpoint (https://certification-api.iota.org/docs/?javascript--nodejs#createupdatetemplate) with your credentials
        const response = await ax.put(`template`, {
            adminId: ADMIN_USER_ID,
            adminPassPhrase: ADMIN_PASS_PHRASE,
            name: `My test certificate`,
            backgroundId,
            additionalContent: `Test Certificate`,
            caption: "Template Caption",
            qrColor: "#0000FF",
            canIssue: true
        });

        if (response.data.success) {
            console.log("Created template", response.data.templateId);
            return response.data.templateId;
        } else {
            console.error("Failed creating template", response.data.message);
        }
    }

    async function createIssuer(templateId) {
        console.log("Creating issuer...");
        // Call the issuer endpoint (https://certification-api.iota.org/docs/?javascript--nodejs#createupdateissuer) with your credentials
        const response = await ax.put(`issuer`, {
            adminId: ADMIN_USER_ID,
            adminPassPhrase: ADMIN_PASS_PHRASE,
            name: `test`,
            newPassPhrase: `testPassPhrase`,
            canIssue: true,
            isAdministrator: false,
            allowedTemplates: [templateId],
            signature: `My example signature`
        });

        if (response.data.success) {
            console.log("Created issuer", response.data.issuerId);
            return response.data.issuerId;
        } else {
            console.error("Failed creating issuer", response.data.message);
        }
    }

    async function createCertificate(issuerId, templateId) {
        console.log("Creating certificate...");
        // Call the issuer endpoint (https://certification-api.iota.org/docs/?javascript--nodejs#createcertificate) with your credentials
        const response = await ax.post(`certificate`, {
            issuerId,
            // Specify the text to use to replace the %%%% placeholders in the template
            issuerPassPhrase: `testPassPhrase`,
            participant: `Mr Smith`,
            participationDate: "07/01/2019 - 08/18/2019",
            trainingTitle: `My Course Title`,
            issuedDate: "08/18/2019",
            templateId
        });

        if (response.data.success) {
            console.log("Created certificate", response.data.hash, response.data.validationUrl);
        } else {
            console.error("Failed creating certificate", response.data.message);
        }
    }

    // Create the background, template, issuer, and certificate
    (async function () {
        try {
            const backgroundId = await createBackground();
            if (backgroundId) {
                const templateId = await createTemplate(backgroundId);

                if (templateId) {
                    const issuerId = await createIssuer(templateId);

                    if (issuerId) {
                        await createCertificate(issuerId, templateId);
                    }
                }
            }
        } catch (err) {
            console.error('Request failed', err.message);
        }
    })();
    ```

### 手順4. コードを実行する
<!-- ### Step 4. Run the code -->

次のコマンドを使用して、サンプルコードを実行できます。
<!-- You can run the sample code by using the following command -->

```bash
node index.js
```

次のようなものが表示されるはずです。
<!-- You should see something like the following: -->

```bash
Creating background...
Created background JQHNXOTAT9CBTYGHKANMG9WNCIZ
Creating template...
Created template 9YSCXMHF9TJNYLRSNYZRP9ARVYH
Creating issuer...
Created issuer RAQSQWHEAANVOGWD9RVJI9ZPMHI
Creating certificate...
Created certificate FAZLSJRLEPPBRVITOPBZNTIVEJWPEQARIOEHNMHBEOJDGABLKPYIHWPMAJWVZTJXFIAFGSVXPGOCBQ999 https://certification.iota.works/FAZLSJRLEPPBRVITOPBZNTIVEJWPEQARIOEHNMHBEOJDGABLKPYIHWPMAJWVZTJXFIAFGSVXPGOCBQ999
```

## 次のステップ
<!-- ## Next steps -->

証明書を確認するには、Webブラウザーを開き、コンソールに出力されたURLにアクセスします。
https://certification.iota.works/FAZLSJRLEPPBRVITOPBZNTIVEJWPEQARIOEHNMHBEOJDGABLKPYIHWPMAJWVZTJXFIAFGSVXPGOCBQ999
<!-- To verify your certificate, open a web browser and go to the URL that was printed to the console: https://certification.iota.works/FAZLSJRLEPPBRVITOPBZNTIVEJWPEQARIOEHNMHBEOJDGABLKPYIHWPMAJWVZTJXFIAFGSVXPGOCBQ999 -->

![Test certificate](../images/test-certificate.png)

[管理ポータル](https://certification-admin.iota.org/)を使用して、新しい証明書を作成するか、既存の証明書を確認します。
<!-- Use the [administration portal](https://certification-admin.iota.org/) to create new certificates or check for existing ones. -->


[API](https://certification-api.iota.org/docs/#iota-certification-api)を使用して、証明書を管理します。たとえば、証明書を間違えて無効にしたい場合は、[`setBlacklist`エンドポイント](https://certification-api.iota.org/docs/#setblacklist)を使用します。
<!-- Use the [API](https://certification-api.iota.org/docs/#iota-certification-api) to manage your certificates. For example, if you make a mistake on a certificate and want to invalidate it, use the [`setBlacklist` endpoint](https://certification-api.iota.org/docs/#setblacklist). -->
