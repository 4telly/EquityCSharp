
# EquityCSharp // EquityBeta

Ethereum wallet miner sources written in c#


## Build your Miner

#### NuGet Packages you need
- Nethereum.Web3

This repository does not include: License Server code, Windows Forms examples

##### **Generate random HEX**
```csharp
public static string GetRandomHexNumber(int digits)
{
    byte[] buffer = new byte[digits / 2];
    random.NextBytes(buffer);
    string result = String.Concat(buffer.Select(x => x.ToString("X2")).ToArray());
    if (digits % 2 == 0)
        return result;
    return result + random.Next(16).ToString("X");
}
```
Get you're private key via `var address = GetRandomHexNumber(64);` then convert the private key to an Public Adress, seems like this => 

`var key = new EthECKey(privateKey.HexToByteArray(), true);`

after this you just need to call the amount of the wallet with web3.

### **Here is an example**
```csharp
var url = "RPC.example.com";
var web3 = new Web3(url);
var address = equityDLL.GetRandomHexNumber(64);
                                                
                                                
var publicKey = EquityBeta.DarkDesign.GetPublicAddress(address);
var balanceTask = web3.Eth.GetBalance.SendRequestAsync(publicKey);
balanceTask.Wait();
var balance = balanceTask.Result;
```

### **More functions**
The miner is finished but you need add things like check if there is an balance, and if there is, save to file. Also an very important thing is Multi-Threading

## Starting Multi-Threading
The Method we are using is not the best but it's one. To create a thread you first need to create the Miner, as an example we use this code:
```csharp
class Miner {
    public static void ThreadProc() {
        try {

            //Miner things

        } catch {

            //if miner fails

        }
    }
}
```
Creating threads is very simple, you just need to define the thread like this;  
`Thread thread = new Thread(new ThreadStart(ThreadProc));` and then execute it via `thread.Start();`. But how we are create now more than one thread, we dont wanna define like 100 threads and start them all. The answer is very simple, we looping the execution so many times until we got our wanted thread number.
```csharp
for (int i = 0; i < 10; i++)
{
    Console.WriteLine("Setting up => ThreadProc: {0}", i); //for debugging
    Thread thread = new Thread(new ThreadStart(ThreadProc));
    thread.Start();
}  
```
Nice, next question; How to stop? Easy the miner doing things in the background so we can do things in the foreground. We can modify our Miner so we can stop it outside. For that just define an public boolean like `public static boolean _shouldRun = true;` and then in the Thread you just code `while(_shouldRun) { //miner things }`

For everyone that wanna use Windows Forms and create an Text that auto update checks, you can use this source code:
```csharp
private void automaticReload()
{
    while(true)
    {
        while(_shouldRun)
        {
            System.Threading.Thread.Sleep(500);
            var reload0 = new Form1();
            reload0.Invoke((MethodInvoker)delegate {
                this.materialLabel3.Text = counter.ToString();

                // refresh text
                this.materialLabel3.Refresh();

                // or refresh form
                this.refresh();
            });
        }
    }
}
```

## Authors

- [@4telly](https://www.github.com/4telly)

