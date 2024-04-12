
>interactive between C# and python via socket  

[# python step](#sectionpython)
1. c# python class
```
        public Process PPython;
        public Socket SocketCommunication;
        Action<string> dlgInfoShow;
        byte[] bytes = new byte[100000];

        StartPythonProcess(pythonPath, startScriptPath, startCmd, delPostMsg:anyDelegationAsdlgInfoShow )
                PPython = Process.Start(startInfo);
                PPython.BeginOutputReadLine();
                PPython.OutputDataReceived += new DataReceivedEventHandler(CallBackShow_Received);
                dlgInfoShow = delPostMsg;

        private void CallBackShow_Received(object sender, DataReceivedEventArgs e)
        {
            if (!string.IsNullOrEmpty(e.Data))
            {
                TheMessage = e.Data;                
                if (dlgInfoShow != null)
                    dlgInfoShow(TheMessage);
            }
        }
        public void EstablishToPython(string sAddress, int iPort)//when 1st start python, call this with local ip and port
        {

            SocketCommunication = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp); 
            SocketCommunication.Connect(sAddress, iPort);
            string sTmp = ReceiveMsgFromPython();
            if (!sTmp.Contains("accepted"))//get response from python side
            {
                throw new Exception($"Invalid response from python: {sTmp}");
            }
        }
        public string TxRxPython(string sMsg)//for interact with python
        {
            SendToPython(sMsg);
            return ReceiveFromPython();
        }
        private string ReceiveFromPython(int iTimeOutInMM = 1000)
        {
            string sTmp;
            if (SocketCommunication.Connected)
            {
                SocketCommunication.ReceiveTimeout = iTimeOutInMM;
                var iRet = SocketCommunication.Receive(bytes);
                sTmp = Encoding.UTF8.GetString(bytes, 0, iRet);
            }
            else
            {
                throw new Exception("Python connection lost!");
            }
            return sTmp;
        }

        private void SendToPython(string sMsg)
        {
            int iSize = Encoding.UTF8.GetBytes(sMsg).Length;
            int iLen = sMsg.Length;
            sMsg = "msgsize" + iSize.ToString() + "/" + iLen + "#:" + sMsg;
            if (SocketCommunication.Connected)
            {
                SocketCommunication.SendTimeout = 1000;
                SocketCommunication.Send(Encoding.UTF8.GetBytes(sMsg));
            }
            else
            {
                throw new Exception("Python connection lost!");
            }
        }
```

2. negotiation command(protocal)  
format such as "commandhead --locale en_US(language setting) (other command) --callfromCsharp --socket_port 50000 50001" 

3. use do loop to monitor python report events/interact
>here can predefine command header to distinguish any events
>even send stop python command or get exceptions report 


<a id="sectionpython"></a>

4. python side
>

a. python script can run without any dependency from C#, thus can called by 2 in the first  

```
  event_cs                    =  threading.Event()
```

  b. use do loop for event monitoring, typically as   
        
```
    def run(self):
        self.event_monitor = threading.Event()
        self.test_timer.enter()
        while True:
                _status = self.check_command()#get and check any command, prepare next status [^1]
                #do anything here, exit loop when finish or aborted
        #anything need done after running
        self.test_timer.exit()
        event_cs.set()
        self.event_monitor.set()
        self.socket_csharp.close()
```

[^1]: this can start monitor event as below:
```
def start_monitor_fromCsharp(self):

        self.socket_port = self.args.socket_port_fromCSharp
        if self.socket_port[0] == '':
            raise Exception(f'No socket_port_fromCSharp configured')
        global thread_monitor
        thread_monitor = threading.Thread(target=self.monitor_external, args=(self.event_monitor,))
        thread_monitor.start()
        print('StartServerForCsharp') # will be accepted by Csharp side
        while not self.ccsharp_connected:
            print('wait connecting...', flush=True)
            time.sleep(0.1)
def monitor_external(self, event: threading.Event):
        HOST = '127.0.0.1'  #server hostname or IP 
        PORT = int(self.socket_port[0])        # The port used by C# server
        buf_size = 100000
        self.server_cs = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.server_cs.bind((HOST, PORT))
        self.server_cs.listen(100)
        print(f'StartServerForCS {PORT}', flush=True) # will be accepted by cs
        self.socket_csharp, addr = self.server_cs.accept()
        print(f'Accept connection from address {addr}', flush=True)
        self.socket_csharp.sendall('Accepted.'.encode('utf-8'))
        # will be accepted by CS, socket successfully here

        while not event_cs.is_set():
                msg:str = self.socket_csharp.recv(buf_size).decode('utf-8')
                #anything need do here, else can send back to cs side for command verification or something other
                self.socket_csharp.sendall(msg.encode('utf-8'))


```