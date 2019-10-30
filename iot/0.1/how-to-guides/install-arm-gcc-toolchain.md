# ARM GCCツールチェーンをインストールする
<!-- # Install the ARM GCC toolchain -->

**ARMツールチェーンを使用すると、マイクロコントローラーで実行できるバイナリにコードをコンパイルできます。**
<!-- **The ARM toolchain allows you to compile code into binary that your microcontroller can run.** -->

1. [最新のARMツールチェーンをダウンロードします](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads)。
<!-- 1. [Download the latest ARM toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) -->

2. 古いARMツールチェーンを削除します。
  <!-- 2. Remove the old ARM toolchain -->

    ```bash
    sudo apt remove binutils-arm-none-eabi gcc-arm-none-eabi libnewlib-arm-none-eabi
    ```

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

6. ファイルの下部で、パスをARMツールチェーンの`bin`ディレクトリに追加します。
  <!-- 6. At the bottom of the file, add the path to the ARM toolchain's `bin` directory -->

    ```bash
    export PATH=$PATH:~/.local/bin/:/opt/GCC_ARM_DIRECTORY_NAME/bin
    ```
