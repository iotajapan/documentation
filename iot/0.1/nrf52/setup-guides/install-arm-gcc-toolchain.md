# ARM GCCツールチェーンをインストールする
<!-- # Install the ARM GCC toolchain -->

**ARMツールチェーンを使用すると、マイクロコントローラーで実行できるマシンコードにコードをコンパイルできます。**
<!-- **The ARM toolchain allows you to compile code into machine code that your microcontroller can run.** -->

## ハードウェア
<!-- ## Hardware -->

このガイドを完了するには、Linux PCが必要です。
<!-- To complete this guide, you need a Linux PC. -->

---

1. 既存のARMツールチェーンを削除します。
  <!-- 1. Remove any existing ARM toolchain -->

    ```bash
    sudo apt remove binutils-arm-none-eabi gcc-arm-none-eabi libnewlib-arm-none-eabi
    ```

2. [最新のARMツールチェーンをダウンロードします](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads)。
<!-- 2. [Download the latest ARM toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) -->

3. 新しいARMツールチェーンを展開します。`NAME_AND_VERSION_OF_TOOLCHAIN`プレースホルダーを、ダウンロードしたツールチェーンファイルの名前に置き換えます。
  <!-- 3. Untar the new ARM toolchain. Replace the `NAME_AND_VERSION_OF_TOOLCHAIN` placeholder with the name of the toolchain file you downloaded -->

    ```bash
    tar -xjvf NAME_AND_VERSION_OF_TOOLCHAIN.tar.bz2
    ```

4. ツールチェーンを`/opt/`ディレクトリに移動します。`GCC_ARM_DIRECTORY_NAME`プレースホルダーを、ツールチェーンを展開したディレクトリに置き換えます。
  <!-- 4. Move the toolchain to the `/opt/` directory. Replace the `GCC_ARM_DIRECTORY_NAME` placeholder with the directory where you untarred the toolchain -->

    ```bash
    sudo mv GCC_ARM_DIRECTORY_NAME/ /opt/
    ```

5. `$HOME`ディレクトリにある`.bashrc`ファイルを開きます。
  <!-- 5. Open the `.bashrc` file in your `$HOME` directory -->

    ```bash
    sudo nano .bashrc
    ```

6. ファイルの下部で、ARMツールチェーンの`bin`ディレクトリを`PATH`変数に追加します。
  <!-- 6. At the bottom of the file, add the ARM toolchain's `bin` directory to your `PATH` variable -->

    ```bash
    export PATH=$PATH:~/.local/bin/:/opt/GCC_ARM_DIRECTORY_NAME/bin
    ```

## 次のステップ
<!-- ## Next steps -->

[マイクロコントローラーのセットアップを続けます](../introduction/get-started.md#step-2-set-up-your-development-environment)。
<!-- [Continue setting up your microcontroller](../introduction/get-started.md#step-2-set-up-your-development-environment). -->
