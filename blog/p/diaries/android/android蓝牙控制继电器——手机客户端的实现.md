#手机客户端的实现

---
本文为博主[飞奔的蜗牛](https://huangxiankui.github.io/blog/index.html?diaries/index.md)原创文章，转载请加以说明

---
通过手机自身所带的蓝牙实现连接蓝牙模块，从而实现通过单片机控制继电器，实现弱电控制强电(由之前项目拆分出来用来演示的小demo）
![这里写图片描述](http://img.blog.csdn.net/20151224101358604)
---

首先要实现开启蓝牙设备，以及设置可见时间
```java
{
BluetoothAdapter adapter =BluetoothAdapter.getDefaultAdapter();
if(!adapter.isEnabled())
{
adapter.enable();
}
Intent enable new Intent(BluetoothAdapter.ACTION_REQUEST_DISCOVERABLE);
enable.putExtra(BluetoothAdapter.EXTRA_DISCOVERABLE_DURATION,3600);
startActivity(enable);
Intent searchIntent =new Intent(thia,led.class);
startActivity(searchIntent);
}
```
获得一个BluetoothAdapter适配器，通过getDefaultAdapter()获得系统默认蓝牙适配器。
```java
private static final UUID MY_UUID = UUID.fromString("00001101-0000-1000-8000-00805F9B34FB");
	private static BluetoothAdapter myAdapter=null;
	private ConnectThread myConnectThread;
	private static Handler myHandler;
	private ConnectedThread myConnectedThread;
	
	private static Bluetoothserive mBluetoothserive = new Bluetoothserive();
	
	//private static int mState;
	// Constants that indicate the current connection state
    public static final int STATE_NONE = 0;       // we're doing nothing
    public static final int STATE_LISTEN = 1;     // now listening for incoming connections
    public static final int STATE_CONNECTING = 2; // now initiating an outgoing connection
    public static final int STATE_CONNECTED = 3;  // now connected to a remote device
    private static int mState = STATE_NONE;<pre name="code" class="java">public synchronized void connected(BluetoothSocket socket, BluetoothDevice device) 
	 {
		if(myConnectedThread!=null)
		{
			myConnectedThread.cancel();
			myConnectedThread=null;
		}
		myConnectedThread = new ConnectedThread(socket);
		myConnectedThread.start();
		setState(STATE_CONNECTED);
		Message msg = myHandler.obtainMessage(Kongtiao.MESSAGE_TOAST);
                Bundle bundle = new Bundle();
                bundle.putString(Kongtiao.TOAST, "连接智能盒成功");
                MainActivity.isConnected = true;
                msg.setData(bundle);
               myHandler.sendMessage(msg);
		}
```
实现读写函数
```java
public void write(byte[] out) 
	{
		ConnectedThread r;
		synchronized (this) 
		{
            r = myConnectedThread;
        }
		r.write(out);
	}
public void read(byte[] in)
	{
		ConnectedThread m;
		synchronized (this) 
		{
           m  = myConnectedThread;
        }
	}
Connected类，用于流的处理

private class ConnectedThread extends Thread 
	{
	    private final BluetoothSocket mySocket;
	    private final InputStream myInStream;
	    private final OutputStream myOutStream;
	 
	    public ConnectedThread(BluetoothSocket socket) {
	    	// 这个是之前的用于连接的socket 
	        mySocket = socket;
	        InputStream tmpIn = null;
	        OutputStream tmpOut = null;
	 
	        //从连接的socket里获取InputStream和OutputStream  
	        try {
	            tmpIn = socket.getInputStream();
	            tmpOut = socket.getOutputStream();
	        } catch (IOException e) { }
	 
	        myInStream = tmpIn;
	        myOutStream = tmpOut;
	    }
	 
	  

	public void run() 
	    {
	      byte[] buffer = new byte[1024];  // buffer store for the stream
	     int bytes; // bytes returned from read()
	 
	        // 已经连接上以后持续从通道中监听输入流的情况 
	        while (true) 
	        {
	          try {
	                 //  从通道的输入流InputStream中读取数据到buffer数组中
	                 bytes = myInStream.read(buffer);
	                //  myInStream.read(bytes);
	                // 将获取到数据的消息发送到UI界面，同时也把内容buffer发过去显示
	             myHandler.obtainMessage(Kongtiao.MESSAGE_READ, bytes, -1, buffer)
	                     .sendToTarget();
	                
	            } catch (IOException e) {
	                break;
	            }
	        }
	    }
	 
	   
	    public void write(byte[] bytes) {
	        try {
	        	//将buffer内容写进通道
	            myOutStream.write(bytes);
	            // 用于将自己发送给对方的内容也在UI界面显示  
	        //    mHandler.obtainMessage(BluetoothChat.MESSAGE_WRITE, -1, -1, buffer)  
	        } catch (IOException e) { }
	    }
	 
	    /* //关闭socket，即关闭通道  */
	    public void cancel() {
	        try {
	            mySocket.close();
	            
	        } catch (IOException e) { }
	    }
	}
```
接收以及发送字符的形式，十六进制
```java
// byte write[] = new byte[]{-1,0,0x0f};设置多个十六进制字符，可以有效的防止信号干扰
	// 十六进制数据声明
	byte write[] = new byte[] { 0x0f };
数据处理：

	public final Handler myHandler = new Handler() {

		@Override
		public void handleMessage(Message msg) {
			switch (msg.what) {
			case MESSAGE_TOAST:
				Toast.makeText(getApplicationContext(),
						msg.getData().getString(TOAST), Toast.LENGTH_SHORT)
						.show();
				break;
			case MESSAGE_READ:
				byte[] readBuf = (byte[]) msg.obj;
				//在读别人发过来的数据的时候，由于别人发过来的是一个byte数组，
				//然后数组里面不是每个元素都是有效数据，所以要自己对数据进行String再构造处理
				//第一个参数是字节数组，第二个为偏移量，（内容是从第一个位置写入的），第三个参数是长度。
				break;
					
				} catch (Exception e) {
					// TODO: handle exception
				}
				default:
				break;
			}
			super.handleMessage(msg);
		}

	};
```
Click函数对应的处理
```java
public void onClick(View v) {
			switch (v.getId()) {
			
			// 关闭灯
			case R.id.guan:
				write[0] = 0x02;
                                break;
			// 打开灯
			case R.id.dakaikongtiao:
                              write[0] = 0x01;
                                 break;
                         default:
                        break;
}
```