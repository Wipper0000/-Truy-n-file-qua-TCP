
<h2 align="center">
    <a href="https://dainam.edu.vn/vi/khoa-cong-nghe-thong-tin">
    🎓 Faculty of Information Technology (DaiNam University)
    </a>
</h2>
<h2 align="center">
   TRUYỀN FILE QUA GIAO THỨC TCP
</h2>
<div align="center">
    <p align="center">
        <img src="docs/aiotlab_logo.png" alt="AIoTLab Logo" width="170"/>
        <img src="docs/fitdnu_logo.png" alt="AIoTLab Logo" width="180"/>
        <img src="docs/dnu_logo.png" alt="DaiNam University Logo" width="200"/>
    </p>

[![AIoTLab](https://img.shields.io/badge/AIoTLab-green?style=for-the-badge)](https://www.facebook.com/DNUAIoTLab)
[![Faculty of Information Technology](https://img.shields.io/badge/Faculty%20of%20Information%20Technology-blue?style=for-the-badge)](https://dainam.edu.vn/vi/khoa-cong-nghe-thong-tin)
[![DaiNam University](https://img.shields.io/badge/DaiNam%20University-orange?style=for-the-badge)](https://dainam.edu.vn)

</div>

## 📖 1. Giới thiệu
Trong thời đại công nghệ thông tin phát triển mạnh mẽ, nhu cầu trao đổi và chia sẻ dữ liệu giữa các máy tính ngày càng phổ biến. Một trong những phương thức quan trọng và được sử dụng rộng rãi là truyền file qua giao thức TCP (Transmission Control Protocol).

TCP là giao thức hướng kết nối, đảm bảo tính tin cậy, toàn vẹn và đúng thứ tự dữ liệu trong quá trình truyền. Nhờ đó, việc truyền file qua TCP có thể áp dụng cho nhiều hệ thống thực tế như: dịch vụ FTP, ứng dụng chat, hệ thống lưu trữ dữ liệu phân tán, hay các phần mềm hỗ trợ tải xuống.

🔑 **Các chức năng chính của hệ thống**:

-Kết nối Client – Server qua TCP Socket.

-Truyền và nhận file giữa các client.

-Hiển thị tiến trình truyền và nhận file .

-Lưu file tự động vào thư mục chỉ định.

## 🛠️ 2. Công nghệ sử dụng

**Ngôn ngữ lập trình**: Java

**Giao thức**: TCP,TCP Socket

**Công nghệ giao diện**: Java Swing

**Thư viện**:

-java.net 

-java.io 

**Công cụ phát triển**: Eclipse IDE

**Hệ điều hành**: Windows 11

**JDK**: Java SE


## 🚀 3. Hình ảnh các chức năng


<p align="center">
  <img src="docs/Ảnh 1.png" alt="Ảnh 1" width="500"/>
</p>
<p align="center">
  <em>Hình 1: Giao diện nhập tên user </em>
</p>


<p align="center">
  <img src="docs/Ảnh 3.png" alt="Ảnh 3" width="500"/>
</p>
<p align="center">
  <em>Hình 2: Giao diện chọn file để gửi </em>
</p>


<p align="center">
  <img src="docs/Ảnh 4.png" alt="Ảnh 4" width="500"/>
</p>
<p align="center">
  <em>Hình 3: Giao diện Client sau khi chọn  </em>
</p>


<p align="center">
  <img src="docs/Ảnh 5.png" alt="Ảnh 5" width="500"/>
</p>
<p align="center">
  <em>Hình 4: Giao diện Khi File gửi thành công  </em>
</p>


<p align="center">
  <img src="docs/Ảnh 6.png" alt="Ảnh 6" width="500"/>
</p>
<p align="center">
  <em>Hình 5: Giao diện Client user khác truy cập server sau khi user 1 chuyển file</em>
</p>

## 📝 4. Các bước cài đặt

#### Bước 1: Chuẩn bị môi trường
1. **Kiểm tra Java**: Mở terminal/command prompt và chạy:
   ```bash
   java -version
   javac -version
   ```
   Đảm bảo cả hai lệnh đều hiển thị phiên bản Java 8 trở lên.

2. **Tải mã nguồn**: Sao chép thư mục `Truyen-file-qua-tcp` chứa các file:
   - `FileServer.java`
   - `ChatClient.java`

#### Bước 2: Biên dịch mã nguồn
1.Chạy server:
```bash
java component.FileServer
```

2.Chạy Client:
```bash
java component.ChatClient
```
<p></p>

#### Source Code 

1. FileServer.java:
```package component;

import java.io.*;
import java.net.*;
import java.util.*;

public class FileServer {
    private static final int PORT = 12345;
    private static Set<ClientHandler> clients = Collections.synchronizedSet(new HashSet<>());

    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("✅ FileServer chạy tại port " + PORT);

            while (true) {
                Socket socket = serverSocket.accept();
                ClientHandler clientHandler = new ClientHandler(socket);
                clients.add(clientHandler);
                new Thread(clientHandler).start();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // Gửi file tới tất cả client khác
    public static void broadcastFile(String sender, String fileName, byte[] fileData, ClientHandler self) {
        synchronized (clients) {
            for (ClientHandler client : clients) {
                if (client != self) {
                    client.sendFile(sender, fileName, fileData);
                }
            }
        }
    }

    // Class xử lý client
    static class ClientHandler implements Runnable {
        private Socket socket;
        private DataInputStream dis;
        private DataOutputStream dos;
        private String username;

        public ClientHandler(Socket socket) {
            try {
                this.socket = socket;
                this.dis = new DataInputStream(socket.getInputStream());
                this.dos = new DataOutputStream(socket.getOutputStream());
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        public void run() {
            try {
                // Nhận username từ client
                username = dis.readUTF();
                System.out.println("👤 " + username + " đã kết nối: " + socket.getInetAddress());

                while (true) {
                    String fileName = dis.readUTF();
                    long fileSize = dis.readLong();

                    File dir = new File("server_files");
                    if (!dir.exists()) dir.mkdir();

                    File file = new File(dir, fileName);
                    FileOutputStream fos = new FileOutputStream(file);

                    byte[] buffer = new byte[4096];
                    long remaining = fileSize;
                    int bytesRead;
                    ByteArrayOutputStream baos = new ByteArrayOutputStream();

                    while (remaining > 0 &&
                           (bytesRead = dis.read(buffer, 0, (int) Math.min(buffer.length, remaining))) != -1) {
                        fos.write(buffer, 0, bytesRead);
                        baos.write(buffer, 0, bytesRead);
                        remaining -= bytesRead;
                    }
                    fos.close();

                    System.out.println("📥 " + username + " gửi file: " + fileName);

                    // Gửi tới các client khác
                    FileServer.broadcastFile(username, fileName, baos.toByteArray(), this);
                }
            } catch (IOException e) {
                System.out.println("❌ Mất kết nối với " + username);
            } finally {
                try { socket.close(); } catch (IOException ignored) {}
                clients.remove(this);
            }
        }

        public void sendFile(String sender, String fileName, byte[] fileData) {
            try {
                dos.writeUTF(sender);
                dos.writeUTF(fileName);
                dos.writeLong(fileData.length);
                dos.write(fileData);
                dos.flush();
            } catch (IOException e) {
                System.out.println("⚠️ Lỗi gửi file tới " + username);
            }
        }
    }
}
```

2. ChatClient.java:
```
package component;

import javax.swing.*;
import javax.swing.border.EmptyBorder;
import java.awt.*;
import java.awt.event.*;
import java.io.*;
import java.net.Socket;

public class ChatClient extends JFrame {
    private JButton chooseButton, sendButton;
    private JLabel selectedFileLabel;
    private DefaultListModel<String> chatModel;
    private JList<String> chatList;
    private File selectedFile;
    private Socket socket;
    private DataOutputStream dos;
    private DataInputStream dis;
    private String username;

    public ChatClient(String serverAddress, int port, String username) {
        this.username = username;

        setTitle("File Chat - " + username);
        setSize(500, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        JPanel panel = new JPanel(new BorderLayout());
        panel.setBackground(new Color(245, 245, 255));
        panel.setBorder(new EmptyBorder(10, 10, 10, 10));

        // Danh sách "chat" (file gửi/nhận)
        chatModel = new DefaultListModel<>();
        chatList = new JList<>(chatModel);
        JScrollPane scrollPane = new JScrollPane(chatList);
        scrollPane.setBorder(BorderFactory.createTitledBorder("💬 Lịch sử gửi file"));

        // Panel nút
        JPanel bottomPanel = new JPanel();
        bottomPanel.setBackground(new Color(230, 230, 250));
        chooseButton = new JButton("📂 Chọn file");
        sendButton = new JButton("📤 Gửi");
        sendButton.setEnabled(false);

        // Label hiển thị file đã chọn
        selectedFileLabel = new JLabel("Chưa chọn file nào");
        selectedFileLabel.setForeground(Color.DARK_GRAY);

        bottomPanel.add(chooseButton);
        bottomPanel.add(sendButton);
        bottomPanel.add(selectedFileLabel);

        panel.add(scrollPane, BorderLayout.CENTER);
        panel.add(bottomPanel, BorderLayout.SOUTH);
        add(panel);

        // Chọn file
        chooseButton.addActionListener(e -> {
            JFileChooser fileChooser = new JFileChooser();
            if (fileChooser.showOpenDialog(this) == JFileChooser.APPROVE_OPTION) {
                selectedFile = fileChooser.getSelectedFile();
                selectedFileLabel.setText("Đã chọn: " + selectedFile.getName());
                sendButton.setEnabled(true);
            }
        });

        // Gửi file
        sendButton.addActionListener(e -> {
            if (selectedFile != null) {
                sendFile(selectedFile);
                chatModel.addElement("Bạn: " + selectedFile.getName());
                selectedFileLabel.setText("Chưa chọn file nào"); // reset sau khi gửi
                sendButton.setEnabled(false);
            }
        });

        // Click mở file
        chatList.addMouseListener(new MouseAdapter() {
            public void mouseClicked(MouseEvent evt) {
                if (evt.getClickCount() == 2) {
                    String item = chatList.getSelectedValue();
                    if (item != null && item.contains(": ")) {
                        String fileName = item.substring(item.indexOf(": ") + 2);
                        try {
                            Desktop.getDesktop().open(new File("received/" + fileName));
                        } catch (Exception ex) {
                            JOptionPane.showMessageDialog(null, "Không mở được file!");
                        }
                    }
                }
            }
        });

        // Kết nối server
        try {
            socket = new Socket(serverAddress, port);
            dos = new DataOutputStream(socket.getOutputStream());
            dis = new DataInputStream(socket.getInputStream());

            // Gửi username lên server
            dos.writeUTF(username);

            // Thread nhận file
            new Thread(() -> {
                try {
                    while (true) {
                        String sender = dis.readUTF();
                        String fileName = dis.readUTF();
                        long fileSize = dis.readLong();

                        File dir = new File("received");
                        if (!dir.exists()) dir.mkdir();

                        File file = new File(dir, fileName);
                        FileOutputStream fos = new FileOutputStream(file);

                        byte[] buffer = new byte[4096];
                        long remaining = fileSize;
                        int bytesRead;
                        while (remaining > 0 &&
                               (bytesRead = dis.read(buffer, 0, (int)Math.min(buffer.length, remaining))) != -1) {
                            fos.write(buffer, 0, bytesRead);
                            remaining -= bytesRead;
                        }
                        fos.close();

                        chatModel.addElement(sender + ": " + fileName);
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }).start();

        } catch (Exception e) {
            JOptionPane.showMessageDialog(this, "❌ Không thể kết nối server!");
            System.exit(1);
        }
    }

    private void sendFile(File file) {
        try {
            dos.writeUTF(file.getName());
            dos.writeLong(file.length());

            FileInputStream fis = new FileInputStream(file);
            byte[] buffer = new byte[4096];
            int bytesRead;
            while ((bytesRead = fis.read(buffer)) != -1) {
                dos.write(buffer, 0, bytesRead);
            }
            fis.close();
        } catch (IOException e) {
            JOptionPane.showMessageDialog(this, "⚠️ Lỗi gửi file!");
        }
    }

    public static void main(String[] args) {
        String username = JOptionPane.showInputDialog("Nhập tên của bạn:");
        if (username == null || username.trim().isEmpty()) {
            System.exit(0);
        }
        SwingUtilities.invokeLater(() -> {
            new ChatClient("127.0.0.1", 12345, username).setVisible(true);
        });
    }
}
```
### 🚀 Sử dụng ứng dụng

1. **Kết nối server**: chạy Server.java để lắng nghe trên port (ví dụ 5001).
2. **Kết nối Client**: chạy Client.java và nhập IP, port server để kết nối.
3. **Chọn File**: client chọn file cần gửi từ máy tính.
4. **Gửi File**: client gửi file qua socket TCP tới server.
5. **Nhận File**: server nhận dữ liệu và ghi file vào thư mục đích.
6. **Lưu File**: file được lưu tự động trong thư mục mặc định của server.
7. **Ngắt kết nối**:client thoát ứng dụng hoặc nhấn Ctrl+C để dừng.

© 2025 AIoTLab, Faculty of Information Technology, DaiNam University. All rights reserved.

---
