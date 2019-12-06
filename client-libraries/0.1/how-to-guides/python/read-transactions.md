# Read transactions on the Tangle in Python

**In this guide, you get [transactions](root://getting-started/0.1/transactions/transactions.md) from the Tangle by connecting to a [node](root://getting-started/0.1/network/nodes.md) and asking it to filter them by their bundle hash. Then, you decode the message in the transaction and print it to the console.**

## Packages

To complete this guide, you need to install the following package:

```bash
pip install pyota
```

## IOTA network

In this guide, we connect to a node on the [Devnet](root://getting-started/0.1/network/iota-networks.md#devnet).

## Code walkthrough

1. Import the packages

    ```python
    from iota import Iota
    from iota import Transaction
    ```

2. Connect to a node

    ```python
    api = Iota('https://nodes.devnet.iota.org:443', testnet = True)
    ```

3. Define the bundle hash that you want to use to filter transactions

    ```python
    bundle = 'IYPHGPIAO99XFAIBRXB9BEQLTZBCXTAGHUXL9UUXGGHHNKEBVEANQIBOALKSO9KLHTEEZXXPB9IOBK9RB'
    ```

4. Use the [`find_transaction_objects()`](https://pyota.readthedocs.io/en/latest/api.html#find-transaction-objects) method to get transactions by the value of their `bundle` field

    ```python
    transactions = api.find_transaction_objects(bundles=[bundle])
    ```

5. Convert the message in the bundle's first transaction to ASCII characters and print it to the console

    ```python
    # Note, that the python api returns a python dictionary with a 'transactions'
    # key and a list of transaction objects as value for it.
    message = transactions['transactions'][0].signature_message_fragment
    print(message.decode())
    ```

    In the console, you should see your message:

    ```
    Hello world
    ```

:::success:Congratulations :tada:
You've just found and read a transaction on the Tangle.
:::

## Run the code

We use the [REPL.it tool](https://repl.it) to allow you to run sample code in the browser.

Click the green button to run the sample code in this guide and see the results in the window.

<iframe height="600px" width="100%" src="https://repl.it/@jake91/Read-a-transaction-on-the-Tangle-Python?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

## Next steps

[Generate a new address](../python/generate-an-address.md).

