# Computer-Network-lab
ex 2

import java.io.FileOutputStream;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.URL;

public class Download {

    public static void main(String[] args) {

        try {
            String fileName = "digital_image_processing.jpg";
            String website = "http://tutorialspoint.com/java_dip/images/" + fileName;

            System.out.println("Downloading File From: " + website);

            URL url = new URL(website);
            InputStream inputStream = url.openStream();
            OutputStream outputStream = new FileOutputStream(fileName);

            byte[] buffer = new byte[2048];
            int length;

            while ((length = inputStream.read(buffer)) != -1) {
                System.out.println("Buffer Read of length: " + length);
                outputStream.write(buffer, 0, length);
            }

            inputStream.close();
            outputStream.close();

            System.out.println("Download Completed Successfully.");

        } catch (Exception e) {
            System.out.println("Exception: " + e.getMessage());
        }
    }
}



Exp 3 a

 import java.net.*;
import java.io.*;

public class Server {
    public static final int PORT = 4000;

    public static void main(String args[]) {
        try (ServerSocket sersock = new ServerSocket(PORT)) {
            System.out.println("Server Started: " + sersock);
            
            // Wait for client connection
            try (Socket sock = sersock.accept()) {
                System.out.println("Client Connected: " + sock);
                
                // Read from client using BufferedReader (standard practice)
                BufferedReader ins = new BufferedReader(new InputStreamReader(sock.getInputStream()));
                System.out.println("Client says: " + ins.readLine());
                
                // Write to client
                PrintWriter ios = new PrintWriter(sock.getOutputStream(), true);
                ios.println("Hello from server");
                
                System.out.println("Connection from: " + sock.getInetAddress());
            } 
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
[10:16 pm, 17/2/2026] 🩶: import java.io.*;
import java.net.*;

public class Client {
    public static void main(String args[]) {
        Socket sock = null;
        System.out.println("Trying to connect...");
        
        try {
            // Connect to localhost on Server's PORT
            sock = new Socket(InetAddress.getLocalHost(), 4000);
            
            // Send message to server
            PrintWriter ps = new PrintWriter(sock.getOutputStream(), true);
            ps.println("Hi from client");

            // Receive message from server
            BufferedReader is = new BufferedReader(new InputStreamReader(sock.getInputStream()));
            System.out.println("Server says: " + is.readLine());
            
        } catch (SocketException e) {
            Sys…

Exp 3 b


import java.io.*;
import java.net.*;

class tcpchatserver {
    public static void main(String args[]) throws Exception {
        PrintWriter toClient;
        BufferedReader fromUser, fromClient;
        try {
            ServerSocket Srv = new ServerSocket(4000);
            System.out.println("Server started... waiting for client");
            
            Socket Clt = Srv.accept();
            System.out.println("Client connected");

            toClient = new PrintWriter(new BufferedWriter(new OutputStreamWriter(Clt.getOutputStream())), true);
            fromClient = new BufferedReader(new InputStreamReader(Clt.getInputStream()));
            fromUser = new BufferedReader(new InputStreamReader(System.in));

            String CltMsg, SrvMsg;
            while (true) {
                CltMsg = fromClient.readLine();
                if (CltMsg == null || CltMsg.equals("end")) break;

                System.out.println("Client says: " + CltMsg);
                System.out.print("Message to Client: ");
                SrvMsg = fromUser.readLine();
                toClient.println(SrvMsg);
            }

            System.out.println("\nClient Disconnected");
            fromClient.close(); toClient.close(); fromUser.close();
            Clt.close(); Srv.close();
        } catch (Exception E) {
            System.out.println("Error: " + E.getMessage());
        }
    }
}

import java.io.*;
import java.net.*;

class tcpchatclient {
    public static void main(String args[]) throws Exception {
        Socket Clt;
        PrintWriter toServer;
        BufferedReader fromUser, fromServer;
        try {
            Clt = new Socket(InetAddress.getLocalHost(), 4000);
            toServer = new PrintWriter(new BufferedWriter(new OutputStreamWriter(Clt.getOutputStream())), true);
            fromServer = new BufferedReader(new InputStreamReader(Clt.getInputStream()));
            fromUser = new BufferedReader(new InputStreamReader(System.in));

            System.out.println("Connected to Server. Type 'end' to Quit.");
            String CltMsg, SrvMsg;

            while (true) {
                System.out.print("Message to Server: ");
                CltMsg = fromUser.readLine();
                toServer.println(CltMsg);

                if (CltMsg.equals("end")) break;

                SrvMsg = fromServer.readLine();
                System.out.println("Server says: " + SrvMsg);
            }
            Clt.close();
        } catch (Exception E) {
            System.out.println("Error: " + E.getMessage());
        }
    }
}
