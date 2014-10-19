package com.cisco.ifp;


import java.io.IOException;
import java.io.PrintWriter;
import java.sql.* ;  // for standard JDBC programs
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Properties;
import java.util.Set;
import java.math.* ;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebInitParam;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
* Servlet implementation class LoginServlet
*/
@WebServlet(
       description = "Login Servlet", 
       urlPatterns = { "/LoginServlet" })
public class LoginServlet extends HttpServlet {
   private static final long serialVersionUID = 1L;
       int flag=0;Connection conn = null;
       Statement stmt = null;
       HashMap hm = new HashMap();
   public void init() throws ServletException {
	 /*  try {
		Class.forName("oracle.jdbc.driver.oracleDriver");
	} catch (ClassNotFoundException e1) {
		// TODO Auto-generated catch block
		e1.printStackTrace();
	}*/
   	
	   System.out.println("Oracle JDBC Driver Registered!");
	   
//		Connection conn = null;

		try {

			conn = DriverManager.getConnection(
					"jdbc:oracle:thin:@lnxdb-idev3-vm-005.cisco.com:1522:MIGIDEV", "SCMTOOLS",
					"scm#wft00l");

		} catch (SQLException e) {

			System.out.println("Connection Failed! Check output console");
			e.printStackTrace();
			return;

		}

		if (conn!= null) {
			System.out.println("You made it, take control your database now!");
		flag=1;
		
		} else {
			System.out.println("Failed to make connection!");
		}

		db_hash obj=new db_hash();  
	     Thread tobj =new Thread(obj);  
	     tobj.start();  
   }

   class db_hash implements Runnable{  
	   String current_time1=null;
	   public void run(){  
		   try {
			   while(true){
			Thread.sleep(60000);
			Calendar cal1 = Calendar.getInstance();
	    	cal1.getTime();
	    	SimpleDateFormat sdf1 = new SimpleDateFormat("HH:mm:ss");
	    	current_time1= sdf1.format(cal1.getTime());
			System.out.println(current_time1);
			 
			String query = "select count(*) from user_table where TO_PLACE='x'";
			PreparedStatement  pstmt = conn.prepareStatement(query);
		    ResultSet rs = pstmt.executeQuery();
		    if (rs.next())
		    {
			hm.put("Route X", rs.getInt(1)); 
		    }
		    else
		    {
		    	System.out.println("error: could not get the record counts");
		    }
		        	query = "select count(*) from user_table where TO_PLACE='y'";
		        	pstmt = conn.prepareStatement(query);
		        	rs = pstmt.executeQuery();
		        	if (rs.next())
		        	{   	
			hm.put("Route Y", rs.getInt(1)); 
		        	}
		        	else
		        	{
		        		System.out.println("error: could not get the record counts");
		        	}
			 			query = "select count(*) from user_table where TO_PLACE='z'";
			 			pstmt = conn.prepareStatement(query);
			 			rs = pstmt.executeQuery();
			 			if (rs.next())
			        	{   	
			hm.put("Route Z", rs.getInt(1)); 
			        	}
			 			else
			 			{
			 				System.out.println("error: could not get the record counts");
			 			}
			Set set = hm.entrySet(); 
			// Get an iterator 
			Iterator i = set.iterator(); 
			// Display elements 
			while(i.hasNext()) { 
			Map.Entry me = (Map.Entry)i.next(); 
			System.out.print(me.getKey() + ": "); 
			System.out.println(me.getValue()); 
			}
			
			   }
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	     //System.out.println("My thread is in running state.");  
 catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	   }
   }
   protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

       //get request parameters for userID and password
       String from = request.getParameter("from");
       String to = request.getParameter("to");
       String start_time = request.getParameter("start_time");
       String current_time=null;
       
       if(request.getParameter("right_time")!=null)
    	   start_time=null;
        
       PrintWriter out = response.getWriter();
		out.println(from+"  "+to+"  "+start_time);
		if(flag==1)
			out.println("connection successful");
		 
		try {
			stmt = conn.createStatement();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		Calendar cal = Calendar.getInstance();
    	cal.getTime();
    	SimpleDateFormat sdf = new SimpleDateFormat("HH:mm:ss");
    	current_time= sdf.format(cal.getTime());
    	//System.out.println(current_time);

		PreparedStatement updateemp;
		try {
			updateemp = conn.prepareStatement("insert into user_table values(?,?,?,?)");
			 updateemp.setString(1,from);
		      updateemp.setString(2,to);
		      updateemp.setString(3,start_time );
		      updateemp.setString(4,current_time);
		      updateemp.executeUpdate();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
			     
         //  response.sendRedirect("LoginSuccess.jsp");
      
        
   }
   
   public void destroy() {
	   try {
		          stmt.close();
		                conn.close();
		  		        } catch (Exception e) {  }

	  }



}