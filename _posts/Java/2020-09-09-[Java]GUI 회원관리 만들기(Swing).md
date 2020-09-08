---
title: "[Java] GUI 회원관리 만들기(Swing)"
tags:
  - Java
last_modified_at: 2020-09-09T08:06:00-05:00
key: "[Java] GUI 회원관리 만들기(Swing)"
---

# GUI 회원관리 만들기(Swing)

자바 Swing을 이용해 GUI 회원관리를 만들어보자.

<!--more-->

## 프로젝트 설계

![1](/assets/images/200909-1.png)

ServiceLayer가 GUI와 DAO`(Data Access Object)`를 연결해 주고, DAO가 모델인 VO`(Value Object)`를 바탕으로 DBMS와 연결돼있는 구조이다.

## 프로젝트 구조

![2](/assets/images/200909-2.png)

## GUI 디자인

기본적인 디자인은 Window Builder를 이용하여 만들었다.<br>

![3](/assets/images/200909-3.png)

## 싱글턴 Service, DAO

Service와 DAO 객체는 효율적인 구조를 위해 싱글턴으로 생성하였고, DAO는 sql 문법으로 MySQL과 연동된 DB에서 데이터를 출력하는 구조이다.

### 싱글턴 ServiceLayer

![4](/assets/images/200909-4.png)

### 싱글턴 UserDAO

![5](/assets/images/200909-5.png)

### 데이터 출력 sql

![6](/assets/images/200909-6.png)

## 전체 소스

### UserDAO.java

```java
package userService.dao;

import java.sql.Connection;

import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;

import userService.vo.UserVO;

public class UserDAO {
	
	
	private static UserDAO dao = new UserDAO();
	
	private UserDAO() {}
	
	public static UserDAO getInstance() {
		return dao;
	}
	
	public Connection getConnection() {
		Connection conn = null;
		try {
			Class.forName("com.mysql.cj.jdbc.Driver");
			conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/webdb?serverTimezone=UTC", "root",
					"1234567890");
		}catch(Exception e) {
			System.out.println(e);
			return null;
		}
		return conn;
	}
	
	public void close(Connection conn, PreparedStatement ps, ResultSet rs) {
		if (rs != null) {
			try {
				rs.close();
			} catch (Exception e) {
				System.out.println("오류 발생 : " + e);
			}
		}
		close(conn, ps);
	}
	
	public void close(Connection conn, PreparedStatement ps) {
		if (ps != null) {
			try {
				ps.close();
			} catch (Exception e) {
				System.out.println("오류 발생 : " + e);
			}
		}
		if (conn != null) {
			try {
				conn.close();
			} catch (Exception e) {
				System.out.println("오류 발생 : " + e);
			}
		}
	}
	
	
	public String[][] userList() {
		ArrayList<String[]> list = new ArrayList<String[]>();
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;

		try {
			conn = getConnection();
			pstmt = conn.prepareStatement("select * from user order by id desc;");
			rs = pstmt.executeQuery();
				while(rs.next()) {
					list.add(new String[] {
							Integer.toString(rs.getInt(1)),
							rs.getString(2),
							Integer.toString(rs.getInt(3)),
							rs.getString(4)
					});
				}
		} catch (Exception e) {
			System.out.println("오류 발생 : " + e);
		} finally {
			close(conn, pstmt, rs);
		}
		String[][] arr = new String[list.size()][4];
		return list.toArray(arr);
	}
	
	public String[][] findByName(String name) {
		ArrayList<String[]> list = new ArrayList<String[]>();
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;

		try {
			conn = getConnection();
			pstmt = conn.prepareStatement("select * from user where name like '%"+name+"%' order by id desc;");
			rs = pstmt.executeQuery();
				while(rs.next()) {
					list.add(new String[] {
							Integer.toString(rs.getInt(1)),
							rs.getString(2),
							Integer.toString(rs.getInt(3)),
							rs.getString(4)
					});
				}
		} catch (Exception e) {
			System.out.println("오류 발생 : " + e);
		} finally {
			close(conn, pstmt, rs);
		}
		String[][] arr = new String[list.size()][4];
		return list.toArray(arr);
	}
	
	
	public void userInsert(UserVO user) {
		Connection conn = null;
		PreparedStatement pstmt = null;
		try {
			conn = getConnection();
			pstmt = conn.prepareStatement("insert into user(name,birth,number) values(?,?,?)");
			pstmt.setString(1, user.getName());
			pstmt.setInt(2, user.getBirth());
			pstmt.setString(3, user.getNumber());
			pstmt.executeUpdate();
		} catch (Exception e) {
			System.out.println("오류 발생 : " + e);
		} finally {
			close(conn, pstmt);
		}
	}
	
	public void update(int id, String number) {
		Connection conn = null;
		PreparedStatement pstmt = null;
		try {
			conn = getConnection();
			pstmt = conn.prepareStatement("update user set number=? where id=?");
			pstmt.setString(1, number);
			pstmt.setInt(2, id);
			pstmt.executeUpdate();
		} catch (Exception e) {
			System.out.println("오류 발생 : " + e);
		} finally {
			close(conn, pstmt);
		}
	}
	
	public void delete(int id) {
		Connection conn = null;
		PreparedStatement pstmt = null;
		try {
			conn = getConnection();
			pstmt = conn.prepareStatement("delete from user where id=?");
			pstmt.setInt(1, id);
			pstmt.executeUpdate();
		} catch (Exception e) {
			System.out.println("오류 발생 : " + e);
		} finally {
			close(conn, pstmt);
		}
	}

}
```

### UserService.java (GUI)

```java
package userService.gui;

import java.awt.EventQueue;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.SwingConstants;

import userService.serviceLayer.ServiceLayer;
import userService.vo.UserVO;

public class UserService {

	ServiceLayer serviceLayer = ServiceLayer.getInstance();
	private JFrame frame;
	private JTextField name;
	private JTextField birth;
	private JTextField number;
	private JTextField findName;
	private JTextField modifyNum;
	private JTextField modifyId;
	private JTextField deleteId;

	/**
	 * Launch the application.
	 */
	public static void main(String[] args) {
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					UserService window = new UserService();
					window.frame.setVisible(true);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
	}

	/**
	 * Create the application.
	 */
	public UserService() {
		initialize();
	}

	/**
	 * Initialize the contents of the frame.
	 */
	private void initialize() {
		frame = new JFrame();
		frame.setBounds(100, 100, 784, 615);
		frame.setLocationRelativeTo(null);
		frame.setResizable(false);
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		frame.getContentPane().setLayout(null);
		
		JLabel NameLabel = new JLabel("name :");
		NameLabel.setBounds(75, 27, 62, 18);
		frame.getContentPane().add(NameLabel);
		
		JLabel BirthLabel = new JLabel("birth :");
		BirthLabel.setBounds(270, 27, 62, 18);
		frame.getContentPane().add(BirthLabel);
		
		JLabel NumberLabel = new JLabel("number :");
		NumberLabel.setHorizontalAlignment(SwingConstants.TRAILING);
		NumberLabel.setBounds(444, 27, 62, 18);
		frame.getContentPane().add(NumberLabel);
		
		name = new JTextField();
		name.setBounds(124, 24, 124, 24);
		frame.getContentPane().add(name);
		name.setColumns(10);
		
		birth = new JTextField();
		birth.setBounds(311, 24, 116, 24);
		frame.getContentPane().add(birth);
		birth.setColumns(10);
		
		number = new JTextField();
		number.setBounds(510, 24, 116, 24);
		frame.getContentPane().add(number);
		number.setColumns(10);
		
		JButton save = new JButton("\uB4F1\uB85D");
		save.setBounds(640, 23, 105, 27);
		frame.getContentPane().add(save);
		
		JLabel FindNameLabel = new JLabel("Find Name :");
		FindNameLabel.setHorizontalAlignment(SwingConstants.TRAILING);
		FindNameLabel.setBounds(167, 75, 138, 18);
		frame.getContentPane().add(FindNameLabel);
		
		findName = new JTextField();
		findName.setBounds(311, 72, 116, 24);
		frame.getContentPane().add(findName);
		findName.setColumns(10);
		
		JButton find = new JButton("\uC870\uD68C");
		find.setBounds(441, 71, 105, 27);
		frame.getContentPane().add(find);
		
		JButton findAll = new JButton("\uC804\uCCB4 \uD68C\uC6D0 \uBAA9\uB85D");
		findAll.setBounds(311, 228, 159, 27);
		frame.getContentPane().add(findAll);
		
		JTextArea ta = new JTextArea();
		ta.setBounds(180, 267, 446, 301);
		frame.getContentPane().add(ta);
		
		ta.setText("");
		String[][] arr = serviceLayer.userList();
		
		ta.append("number"+"\t"+"name"+"\t"+"birth date"+"\t"+"phone number"+"\n");
		ta.append("--------------------------------------------------------------------------------------------------------------\n");
		
		for (int i = 0; i < arr.length; i++) {
			ta.append(arr[i][0]+" \t "+ arr[i][1]+" \t "+arr[i][2]+" \t "+arr[i][3]+" \t "+ "\n");
		}
		
		
		modifyNum = new JTextField();
		modifyNum.setBounds(401, 127, 116, 24);
		frame.getContentPane().add(modifyNum);
		modifyNum.setColumns(10);
		
		modifyId = new JTextField();
		modifyId.setBounds(180, 127, 116, 24);
		frame.getContentPane().add(modifyId);
		modifyId.setColumns(10);
		
		JLabel numberLabel2 = new JLabel("number :");
		numberLabel2.setHorizontalAlignment(SwingConstants.TRAILING);
		numberLabel2.setBounds(325, 130, 62, 18);
		frame.getContentPane().add(numberLabel2);
		
		JLabel idLabel = new JLabel("ID :");
		idLabel.setBounds(134, 130, 62, 18);
		frame.getContentPane().add(idLabel);
		
		JButton modifyBtn = new JButton("\uC218\uC815");
		modifyBtn.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
			}
		});
		modifyBtn.setBounds(559, 126, 105, 27);
		frame.getContentPane().add(modifyBtn);
		
		JLabel label = new JLabel("ID :");
		label.setBounds(292, 174, 62, 18);
		frame.getContentPane().add(label);
		
		deleteId = new JTextField();
		deleteId.setBounds(335, 171, 116, 24);
		frame.getContentPane().add(deleteId);
		deleteId.setColumns(10);
		
		JButton deleteBtn = new JButton("\uC0AD\uC81C");
		deleteBtn.setBounds(477, 170, 105, 27);
		frame.getContentPane().add(deleteBtn);
		
				

		

		

		
		findAll.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent arg0) {
				
				ta.setText("");
				String[][] arr = serviceLayer.userList();
				
				ta.append("number"+"\t"+"name"+"\t"+"birth date"+"\t"+"phone number"+"\n");
				ta.append("--------------------------------------------------------------------------------------------------------------\n");
				
				for (int i = 0; i < arr.length; i++) {
					ta.append(arr[i][0]+" \t "+ arr[i][1]+" \t "+arr[i][2]+" \t "+arr[i][3]+" \t "+ "\n");
				}
				
			}
		});
		
		
		
		
		find.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				String val = findName.getText();
				
				ta.setText("");
				String[][] arr = serviceLayer.findByName(val);
				if(arr.length==0) JOptionPane.showMessageDialog(null, "해당되는 사용자가 없습니다.");
				
				ta.append("number"+"\t"+"name"+"\t"+"birth date"+"\t"+"phone number"+"\n");
				ta.append("--------------------------------------------------------------------------------------------------------------\n");
				
				for (int i = 0; i < arr.length; i++) {
					ta.append(arr[i][0]+" \t "+ arr[i][1]+" \t "+arr[i][2]+" \t "+arr[i][3]+" \t "+ "\n");
				}
			}
		});
		
		save.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent arg0) {
				String nameValue = name.getText();
				String birthValue = birth.getText();
				String numberValue = number.getText();
				
				if( nameValue.isEmpty() || birthValue.isEmpty() || numberValue.isEmpty()) {
					JOptionPane.showMessageDialog(null, "모두 작성해주세요.");
				}
				else {
					UserVO user = new UserVO();
					user.setName(nameValue);
					user.setBirth(Integer.parseInt(birthValue));
					user.setNumber(numberValue);
					
					ServiceLayer serviceLayer = ServiceLayer.getInstance();
					serviceLayer.userInsert(user);
					
					JOptionPane.showMessageDialog(null, "등록되었습니다.");
					
					ta.setText("");
					String[][] arr = serviceLayer.userList();
					
					ta.append("number"+"\t"+"name"+"\t"+"birth date"+"\t"+"phone number"+"\n");
					ta.append("----------------------------------------------------------------------------------------------------------\n");
					
					for (int i = 0; i < arr.length; i++) {
						ta.append(arr[i][0]+" \t "+ arr[i][1]+" \t "+arr[i][2]+" \t "+arr[i][3]+" \t "+ "\n");
					}
									
				}
			}
		});
		
		modifyBtn.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent arg0) {
				String idValue = modifyId.getText();
				String numberValue = modifyNum.getText();
				
				if( idValue.isEmpty() || numberValue.isEmpty()) {
					JOptionPane.showMessageDialog(null, "모두 작성해주세요.");
				}
				else {
					ServiceLayer serviceLayer = ServiceLayer.getInstance();
					serviceLayer.update(Integer.parseInt(idValue), numberValue);
					
					JOptionPane.showMessageDialog(null, "수정이 완료되었습니다.");
					
					ta.setText("");
					String[][] arr = serviceLayer.userList();
					
					ta.append("number"+"\t"+"name"+"\t"+"birth date"+"\t"+"phone number"+"\n");
					ta.append("--------------------------------------------------------------------------------------------------------------\n");
					
					for (int i = 0; i < arr.length; i++) {
						ta.append(arr[i][0]+" \t "+ arr[i][1]+" \t "+arr[i][2]+" \t "+arr[i][3]+" \t "+ "\n");
					}
					
				}
			}
		});
		
		deleteBtn.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent arg0) {
				String idValue = deleteId.getText();
				
				if( idValue.isEmpty()) {
					JOptionPane.showMessageDialog(null, "Id를 작성해주세요.");
				}
				else {
					ServiceLayer serviceLayer = ServiceLayer.getInstance();
					serviceLayer.delete(Integer.parseInt(idValue));
					
					JOptionPane.showMessageDialog(null, "삭제가 완료되었습니다.");
					
					ta.setText("");
					String[][] arr = serviceLayer.userList();
					
					ta.append("number"+"\t"+"name"+"\t"+"birth date"+"\t"+"phone number"+"\n");
					ta.append("--------------------------------------------------------------------------------------------------------------\n");
					
					for (int i = 0; i < arr.length; i++) {
						ta.append(arr[i][0]+" \t "+ arr[i][1]+" \t "+arr[i][2]+" \t "+arr[i][3]+" \t "+ "\n");
					}
				}
			}
		});
	}
```

### ServiceLayer.java

```java
package userService.serviceLayer;

import java.util.ArrayList;


import userService.dao.UserDAO;
import userService.vo.UserVO;

public class ServiceLayer {
	private static ServiceLayer serviceLayer = new ServiceLayer();
	
	private ServiceLayer() { }
	
	public UserDAO dao = UserDAO.getInstance();
	
	public static ServiceLayer getInstance() {
		return serviceLayer;
	}
	
	public boolean userInsert(UserVO user) {
		dao.userInsert(user);
		return true;
	}
	
	public String[][] userList(){
		String[][] list = dao.userList();
		return list;
	}
	
	public String[][] findByName(String name){
		String[][] list = dao.findByName(name);
		return list;
	}
	
	public boolean update(int id, String number) {
		dao.update(id, number);
		return true;
	}
	
	public boolean delete(int id) {
		dao.delete(id);
		return true;
	}
}
```

### UserVO.java

```java
package userService.vo;

public class UserVO {
	private int id;
	private String name;
	private int birth;
	private String number;
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getBirth() {
		return birth;
	}
	public void setBirth(int birth) {
		this.birth = birth;
	}
	public String getNumber() {
		return number;
	}
	public void setNumber(String number) {
		this.number = number;
	}
	
	
}
```