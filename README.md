# sample-stock-management-system


//in grid1 file
import java.awt.*;
import javax.swing.*;
import javax.swing.event.AncestorEvent;
import javax.swing.event.AncestorListener;
import java.awt.event.*;
public class Grid1
{
    JFrame frameObj;
    Grid1(String username)
    {
        frameObj = new JFrame("Welcome");
        Toolkit tk = Toolkit.getDefaultToolkit(); //Initializing the Toolkit class.
        Dimension screenSize = tk.getScreenSize(); //Get the Screen resolution of our device.
        frameObj.setSize(screenSize.width,screenSize.height);
        JPanel p = new JPanel();
        JLabel m = new JLabel("STOCK MANAGEMENT MENU ");

        m.setBounds(10,10,200,100);

        m.setForeground(Color.white);
        m.setFont(new Font("serif",40,40));
        p.add(m);

        JButton btn1 = new JButton("Stock Buying");
        JButton btn2 = new JButton("Stock Selling");
        JButton btn3 = new JButton("Menu");
        JButton btn4 = new JButton("Stock Tracing");
        btn1.setBounds(150,150,200,40);

        btn2.setBounds(150,200,200,40);
        btn4.setBounds(150,250,200,40);
        p.setBounds(0,0,screenSize.width,80);
        p.setBackground(Color.black);
        frameObj.add(p);
        frameObj.add(btn1);
        frameObj.add(btn2);
        frameObj.add(btn4);
        frameObj.add(btn3);
       btn3.setVisible(false);
        btn1.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                new StockBuyingFrame(username);
            }
        });
        btn2.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                new StockSelling(username);
            }
        });
        btn4.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                new Stocktracing(username);
            }
        });
        frameObj.setSize(500, 500);
        frameObj.setLocationRelativeTo(null);
        frameObj.setVisible(true);
    }}
   
    
//login backend

import java.sql.*;
public class LoginBackend {

    public static boolean authenticateUser(String user, String pass) {
        try {
            Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/java_project",  "root","rajan" );

            PreparedStatement statement = conn.prepareStatement("SELECT * FROM login_page WHERE Username = ? AND password = ?");
            statement.setString(1, user);
            statement.setString(2, pass);
            ResultSet resultSet = statement.executeQuery();
            if (resultSet.next()) {
                conn.close();
                return true;
            } else {
                conn.close();
                return false;
            }
        } catch (SQLException e) {
            e.printStackTrace();
            return false;
        }
    }
}
//login form
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.lang.Exception;
class LoginForm extends JFrame implements ActionListener
{
    JButton b1;
    JPanel newPanel;
    JLabel username;
    JLabel password;
    final JTextField textField1,textField2;
    LoginForm()
    {
        username = new JLabel();
        username.setText("Username");
        textField1 = new JTextField(15);
        password = new JLabel();
        password.setText("Password");
        textField2 = new JPasswordField(15);
        b1=new JButton("Login");
        newPanel =  new JPanel(new GridLayout(3,1));
        newPanel.add(username);
        newPanel.add(textField1);
        newPanel.add(password);
        newPanel.add(textField2);
        newPanel.add(b1);
        newPanel.setSize(500,200);
        add(newPanel,BorderLayout.CENTER);
        b1.addActionListener(this);
        setTitle("LOGIN PAGE");
    }
    public void actionPerformed(ActionEvent ae)
    {
        String userValue = textField1.getText();
        String passValue = textField2.getText();
        boolean isAuthenticated = LoginBackend.authenticateUser(userValue, passValue);
        if (isAuthenticated) {
           NewPage page = new NewPage(userValue);

            JLabel wel_label = new JLabel("Welcome: "+userValue);
            page.getContentPane().add(wel_label);
            page.setVisible(true);
        }
        else{
            JOptionPane.showMessageDialog(null,"Invalid username and password" , "Error", JOptionPane.ERROR_MESSAGE);
        }
    }
}

public class Loginpage
{
    public static void main(String[] arg)
    {
        try
        {
            LoginForm form = new LoginForm();
            form.setSize(350,200);
            form.setLocationRelativeTo(null);
            form.setVisible(true);
        }
        catch(Exception e)
        {
            JOptionPane.showMessageDialog(null, e.getMessage());
        }
    }
}
//new page 


import javax.swing.*;
import java.awt.event.*;
import java.awt.*;
class NewPage extends JFrame
{
    NewPage(String username)
    {
        JButton btn1 = new JButton("Explore");
        btn1.setBounds(150,215,90,30);
        btn1.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                Grid1 grid1 = new Grid1(username);
            }
        });
        add(btn1);
        setDefaultCloseOperation(javax.swing.WindowConstants.DISPOSE_ON_CLOSE);
        setTitle("Welcome");
        setSize(500, 500);
        setLocationRelativeTo(null);
    }
}



//stock

import java.sql.*;
import javax.swing.*;
public class Stock {

    public void Stocks(String user,String stock,double price){
    try
    {
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/java_project", "root", "rajan");
        PreparedStatement statement = conn.prepareStatement("INSERT INTO information (username,name, price) VALUES (?, ?, ?)");
        statement.setString(1, user);
        statement.setString(2, stock);
        statement.setDouble(3, price);
        statement.executeUpdate();
        statement.close();
        JOptionPane.showMessageDialog(null,"Stock added to "+user);
    }catch(SQLException e){
        e.printStackTrace();
        JOptionPane.showMessageDialog(null,"Stock Not added ");
    }
}
}

//stock1
import java.sql.*;
import javax.swing.*;
public class Stock1{
    public Stock1(String Username) {
        try {
            Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/java_project", "root", "rajan");
            PreparedStatement statement = conn.prepareStatement("delete from information where username=?");
            statement.setString(1, Username);
            statement.executeUpdate();
            statement.close();
            JOptionPane.showMessageDialog(null,"Stock Sell from "+Username);

        }catch(SQLException e){
            e.printStackTrace();
            JOptionPane.showMessageDialog(null,"Stock Not Sell");
        }

    }
}

//stock2

import java.sql.*;
import javax.swing.*;
public class Stock2 {
    int i=0;
    Stock2(String Username){
        JFrame f = new JFrame();
        try {
            Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/java_project", "root", "rajan");
            PreparedStatement statement = conn.prepareStatement("select * from information where username=?");
            statement.setString(1, Username);
            ResultSet rs= statement.executeQuery();
            while(rs.next()&&i<1000){
                String stock=rs.getString("name");
                double price=rs.getDouble("price");
                String price1=String.valueOf(price);
                JLabel l =new JLabel(stock);
                JLabel l1 =new JLabel(price1);
                JButton b =new JButton("Menu");
                l.setBounds(50,10+i,105,30);
                l1.setBounds(300,10+i,105,30);
                f.add(l);
                f.add(l1);
                f.add(b);
                b.setVisible(false);
                f.setSize(500,500);
                f.setVisible(true);
                i +=30;
              //  System.out.println(stock+price);
            }
            statement.close();
        }catch(SQLException e){
            e.printStackTrace();

        }
    }

}

//stock buyinfg form

import javax.swing.*;
import java.awt.*;
public class StockBuyingFrame {

        JFrame f;
        StockBuyingFrame(String username){
        f = new JFrame("Buying");
        Toolkit tk = Toolkit.getDefaultToolkit(); //Initializing the Toolkit class.
        Dimension screenSize = tk.getScreenSize(); //Get the Screen resolution of our device.
        f.setSize(screenSize.width,screenSize.height);
        JPanel p = new JPanel();
        JLabel m = new JLabel("STOCK BUYING ");

        m.setBounds(10,10,200,100);

        m.setForeground(Color.white);
        m.setFont(new Font("serif",40,40));
        p.add(m);
        JLabel l = new JLabel("1.TATA");
        JLabel l1 = new JLabel("2.MAHINDRA");
        JLabel l2 = new JLabel("3.RELIANCE");
        JLabel l3 = new JLabel("4.ADANI SHARES");

        JButton b = new JButton("623.35INR");
        JButton b1 = new JButton("1571.05INR");
        JButton b2 = new JButton("2748.00INR");
        JButton b3 = new JButton("2363.00INR");
        l.setBounds(100, 120, 100, 20);
        l1.setBounds(100, 170, 100, 20);
        l2.setBounds(100, 220, 100, 20);
        l3.setBounds(100, 270, 150, 20);
        p.setBounds(0,0,screenSize.width,80);
        p.setBackground(Color.black);

        b.setBounds(250, 120, 100, 20);
        b1.setBounds(250, 170, 100, 20);
        b2.setBounds(250, 220, 100, 20);
        b3.setBounds(250, 270, 100, 20);
        f.add(p);
        f.add(b);
        f.add(b1);
        f.add(b2);
        f.add(b3);
        f.add(l);
        f.add(l1);
        f.add(l2);
        f.add(l3);
        f.setSize(500, 500);
        f.setLocationRelativeTo(null);
        f.setLayout(null);
        f.setVisible(true);


                Stock controller = new Stock();
                b.addActionListener(e -> controller.Stocks(username,"TATA", 623.35));
                b1.addActionListener(e -> controller.Stocks(username,"MAHINDRA", 1571.05));
                b2.addActionListener(e -> controller.Stocks(username,"RELIANCE", 2748.00));
                b3.addActionListener(e -> controller.Stocks(username,"ADANI SHARES", 2363.00));
    }
    }


//stock selling


import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class StockSelling {
   StockSelling(String Username){
        JFrame f = new JFrame("Selling");
        Toolkit tk = Toolkit.getDefaultToolkit(); //Initializing the Toolkit class.
        Dimension screenSize = tk.getScreenSize(); //Get the Screen resolution of our device.
        f.setSize(screenSize.width,screenSize.height);
        JPanel p = new JPanel();
        JLabel m = new JLabel("STOCK SELLING ");

        m.setBounds(10,10,200,100);

        m.setForeground(Color.white);
        m.setFont(new Font("serif",40,40));
        p.add(m);
        JButton b1 = new JButton("CLICK HERE");
        JLabel l = new JLabel("To Sell your stocks");
        l.setBounds(230,100,190,20);
        b1.setBounds(90,100,120,20);
        b1.addActionListener(new ActionListener() {
             public void actionPerformed(ActionEvent e) {
                new Stock1(Username);
             }
        });
        p.setBounds(0,0,screenSize.width,80);
        p.setBackground(Color.black);
        f.add(p);
        f.add(l);
        f.add(b1);
        f.setSize(500,500);
        f.setLocationRelativeTo(null);
        f.setLayout(null);
        f.setVisible(true);
    }

}



//stock tracking

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Stocktracing {
    Stocktracing(String Username){
        JFrame f = new JFrame("Tracing");
        Toolkit tk = Toolkit.getDefaultToolkit(); //Initializing the Toolkit class.
        Dimension screenSize = tk.getScreenSize(); //Get the Screen resolution of our device.
        f.setSize(screenSize.width,screenSize.height);
        JPanel p = new JPanel();
        JLabel m = new JLabel("STOCK TRACING ");

        m.setBounds(10,10,200,100);

        m.setForeground(Color.white);
        m.setFont(new Font("serif",40,40));
        p.add(m);
        JButton b1 = new JButton("CLICK HERE");
        JLabel l = new JLabel("To Fetch your stocks");
        l.setBounds(230,100,190,20);
        b1.setBounds(90,100,120,20);
        b1.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                new Stock2(Username);
            }
        });
        p.setBounds(0,0,screenSize.width,80);
        p.setBackground(Color.black);
        f.add(p);
        f.add(l);
        f.add(b1);
        f.setSize(500,500);
        f.setLocationRelativeTo(null);
        f.setLayout(null);
        f.setVisible(true);
    }

}

