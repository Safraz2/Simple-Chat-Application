import java.util.ArrayList;
import java.io.PrintWriter;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.Socket;
import java.net.ServerSocket;

public class VerySimpleChatServer {

    ArrayList<PrintWriter> clientOutputStreams;

    public class ClientHandler implements Runnable {
        BufferedReader reader;
        Socket socket;

        public ClientHandler(Socket clientSocket) {
            try {
                socket = clientSocket;
                InputStreamReader isReader = new InputStreamReader(socket.getInputStream());
                reader = new BufferedReader(isReader);
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }

        public void run() {
            String message;
            try {
                while ((message = reader.readLine()) != null) {
                    System.out.println("read: " + message);
                    tellEveryone(message);
                }
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }// close run
    }    // close clientHandler

    public static void main (String[] args) {
        new VerySimpleChatServer().go();
    }//close main

    public void go() {
        clientOutputStreams = new ArrayList<>();
        try {
            ServerSocket serverSocket = new ServerSocket(5000);

            while(true) {
                Socket clientSocket = serverSocket.accept();
                PrintWriter writer = new PrintWriter(clientSocket.getOutputStream());
                clientOutputStreams.add(writer);

                Thread t = new Thread(new ClientHandler(clientSocket));
                t.start();
                System.out.println("Got a connection.");
            }
        } catch (Exception ex) {
            ex.printStackTrace();
        }
    }// close go

    public void tellEveryone(String message) {
        for (PrintWriter pw : clientOutputStreams) {
            try {
                pw.println(message);
                pw.flush();
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }//close for loops
    }// close tellEveryone method
}//close main class

