## Android广播 ##

Android广播能很方便的在组件之间进行通讯

实现方法：



1. 定义类继承*BroadcastReceiver*，重写其*onReceive()*方法

    	private class IsFinishReceiver extends BroadcastReceiver {

        @Override
        public void onReceive(Context context, Intent intent) {
            //接收测试进程关闭广播
            if (intent.getStringExtra("reportLocation") == null){
                String rPackageName = intent.getStringExtra("runPackageName");
                myBinder.getReport(rPackageName);
                software_GridView.setEnabled(false);
                startButton.setText("正在测试");
                startButton.setEnabled(false);
            } else{
                reportLocation = intent.getStringExtra("reportLocation");
                Log.d("reportLocation" , reportLocation);
                startButton.setEnabled(true);
                startButton.setText("测试完成");
                software_GridView.setEnabled(true);
            }

        }
    }
2. 注册广播：
    
    
        IsFinishReceiver isFinishReceiver = new IsFinishReceiver();
        IntentFilter intentFilter = new IntentFilter();
        intentFilter.addAction("cn.testplus.assistant.plugins.unitytest.RECEIVER");
        registerReceiver(isFinishReceiver,intentFilter);

3. 发送广播

    Intent intent = new Intent("cn.testplus.assistant.plugins.unitytest.UploadActivity.RECEIVER");
    intent.putExtra("status",UploadActivity.UPLOAD_BASEINFO_FAILED);
                intent.putExtra("msg",result);    
        