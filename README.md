# Geo-Spatial-Information-System
An information system wherein student will get to know which tram stop is the nearest. This will help students save their time and help them take the trams from school to home.

//importing java libraries
import java.sql.DriverManager;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
 
public class HW22 {
 
	public static Connection connection = null;
	public static void query1(String var1, String var2, String var3, String var4, String var5){
		Statement sta= null;
		ResultSet rs= null;
		try {
			sta = connection.createStatement();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		if(var1.equals("student")){
			//query to extract ids of students within the given dimensions
			try {
				rs = sta.executeQuery("SELECT s.student_id FROM student s WHERE sdo_inside (s.student_location,sdo_geometry (2003, null, null,sdo_elem_info_array (1,1003,3),sdo_ordinate_array ("+var2+","+var3+","+var4+","+var5+"))) = 'TRUE'");
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			try {
				while (rs.next()) {
					  String lastName = rs.getString("STUDENT_ID");
					  System.out.println(lastName + "\n");
					}
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			
			
		}
				else if(var1.equals("building")) {
					//query to list the building ids that are within some distance to a particular student
					try {
						rs = sta.executeQuery("SELECT b.building_id FROM building b WHERE sdo_inside (b.building_location,sdo_geometry (2003, null, null,sdo_elem_info_array (1,1003,3),sdo_ordinate_array ("+var2+","+var3+","+var4+","+var5+"))) = 'TRUE'");
					} catch (SQLException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
					try {
						while (rs.next()) {
							  String lastName = rs.getString("BUILDING_ID");
							  System.out.println(lastName + "\n");
							}
					} catch (SQLException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
					
					
				}
				else if(var1.equals("tramstop")){
					/query to list the tram stop ids that are within some distance to a particular student
					try {
						rs = sta.executeQuery("SELECT t.tramstation_id FROM tramstop t WHERE sdo_inside (t.tramstation_location,sdo_geometry (2003, null, null,sdo_elem_info_array (1,1003,3),sdo_ordinate_array ("+var2+","+var3+","+var4+","+var5+"))) = 'TRUE'"); //query to find the tram stop that cover most of the buildings
					} catch (SQLException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
					try {
						while (rs.next()) {
							  String lastName = rs.getString("TRAMSTATION_ID");
							  System.out.println(lastName + "\n");
							}
					} catch (SQLException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
				}
			}
		
	public static void query2(String var1, String var2){
		Statement sta= null;
		ResultSet rs= null;
		try {
			sta = connection.createStatement();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		// query to list buildings that are within a given particular distance
		try {
			rs = sta.executeQuery("SELECT b.building_id FROM building b, student s WHERE s.student_id = '"+var1+"' AND sdo_within_distance ( s.student_location, b.building_location, 'distance="+var2+"') = 'TRUE'");
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println("BUILDING ID\n");
		try {
			while (rs.next()) {
				  String id = rs.getString("BUILDING_ID");
				  System.out.println(id + "\n");
				}
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		try {
		//query to list tram stops that are within some particular distance
			rs = sta.executeQuery("SELECT t.tramstation_id FROM tramstop t , student s WHERE s.student_id = '"+var1+"' AND sdo_within_distance ( s.student_location, t.tramstation_location, 'distance="+var2+"') = 'TRUE'");
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println("TRAMSTOP ID\n");
		try {
			while (rs.next()) {
				  String id = rs.getString("TRAMSTATION_ID");
				  System.out.println(id + "\n");
				}
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
	}
	
	public static void query3(String var1, String var2, String var3){
	
		Statement sta= null;
		ResultSet rs= null;
		System.out.println(var1 + "\n");
		int var4 = Integer.parseInt(var3);
		var4+=1;
		try {
			sta = connection.createStatement();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		if(var1.equals("student")){
			// select the ids of some nearest students to a given students location
			try {
				rs = sta.executeQuery("SELECT s2.student_id FROM student s, student s2 WHERE s.student_id = '"+var2+"' AND s2.student_id <> '"+var2+"' AND sdo_nn(s2.student_location,s.student_location, 'sdo_num_res="+var4+"') = 'TRUE'");
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			try {
				while (rs.next()) {
					  String id = rs.getString("STUDENT_ID");
					  System.out.println(id + "\n");
					}
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			
			
		}
				else if(var1.equals("building")) {
					// select some buildings which are nearest neighbors of a given building
					try {
						rs = sta.executeQuery("SELECT b2.building_id FROM building b, building b2 WHERE b.building_id = '"+var2+"' AND b2.building_id <> '"+var2+"' AND sdo_nn(b2.building_location,b.building_location, 'sdo_num_res="+var4+"') = 'TRUE'");
					} catch (SQLException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
					try {
						while (rs.next()) {
							  String id = rs.getString("BUILDING_ID");
							  System.out.println(id + "\n");
							}
					} catch (SQLException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
					
					
				}
				else if(var1.equals("tramstop")){
					// select some tram stops which are nearest neighbors of some other tramstop location
					try {
						rs = sta.executeQuery("SELECT t2.tramstation_id FROM tramstop t, tramstop t2 WHERE t.tramstation_id = '"+var2+"' AND t2.tramstation_id <> '"+var2+"' AND sdo_nn(t2.tramstation_location,t.tramstation_location, 'sdo_num_res="+var4+"') = 'TRUE'");
					} catch (SQLException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
					try {
						while (rs.next()) {
							  String id = rs.getString("TRAMSTATION_ID");
							  System.out.println(id + "\n");
							}
					} catch (SQLException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
				}
		
	}
	public static void query4(String var1){
		
		Statement sta= null;
		ResultSet rs= null;
		try {
			sta = connection.createStatement();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		if(var1.equals("2")){
		try {
		//finding the ids of2 nearest tram stops for each student
			rs = sta.executeQuery("SELECT s.student_id, t.tramstation_id FROM tramstop t, student s WHERE sdo_nn(t.tramstation_location,s.student_location, 'sdo_num_res=2') = 'TRUE'");
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println("Student ID\tNearest Neighbours\n");
		try {
			String temp = "";
			while (rs.next()) {
				  String id = rs.getString("STUDENT_ID");
				  String id2 = rs.getString("TRAMSTATION_ID");
				  if(temp.equals(id)){
					  System.out.print(id2 + "\n");
				  }
				  else{
				  System.out.print(id +"\t\t ");
				  System.out.print(id2 + ", ");
				  temp=id;
				}
			}
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		}
		
		else if(var1.equals("4")){
			try {
			// finding the ids of top 5 students that have the most reverse nearest neighbors
				rs = sta.executeQuery("SELECT * from (SELECT s.student_id, count(b.building_id) as build FROM building b, student s WHERE sdo_nn(s.student_location,b.building_location, 'sdo_num_res=1') = 'TRUE' group by s.student_id order by count(b.building_id) desc) where rownum <=5");
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			try {
				System.out.print("Reverse Nearest Neighbors\n");
				System.out.print("Student_Id\t");
				System.out.print("Number of Reverse Nearest Neighbor\n");
				while (rs.next()) {
					  String id = rs.getString("STUDENT_ID");
					  String id2 = rs.getString("build");
						  System.out.print(id + "\t\t\t");
						  System.out.print(id2+ "\n");
				}
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}	
		}
		else if(var1.equals("4")){
			try {
			//quesry to find the id of tram stop that covers most of the buildings
				rs = sta.executeQuery("SELECT t.tramstation_id,count(b.building_id) as build FROM tramstop t, building b WHERE sdo_within_distance ( t.tramstation_location, b.building_location, 'distance=250') = 'TRUE' group by t.tramstation_id order by count(b.building_id) desc");
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			try {
				String temp = "";
				if(rs.next()){
				String id = rs.getString("TRAMSTATION_ID");
				temp = rs.getString("build");
				System.out.print("Tram stop that cover the most buildings: ");	
				System.out.print(id + "\t");				
				}
				while (rs.next()) {
					  String id = rs.getString("TRAMSTATION_ID");
					  String id2 = rs.getString("build");
					  if(temp.equals(id2)){
						  System.out.print(id + "\t");
					  }
				}
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}	
		}
		
	}
	public static void main(String[] argv) {
 
		try {
 
			Class.forName("oracle.jdbc.driver.OracleDriver");
 
		} catch (ClassNotFoundException e) {
 
			System.out.println("Where is your Oracle JDBC Driver?");
			e.printStackTrace();
			return;
 
		}
 
		try {
 
			connection = DriverManager.getConnection(
					"jdbc:oracle:thin:@localhost:1521:orcl1", "system",
					"Sidhant1991");
 
		} catch (SQLException e) {
 
			System.out.println("Connection Failed! Check output console");
			e.printStackTrace();
			return;
 
		}
 
		if (connection != null) {
		} else {
			System.out.println("Failed to make connection!");
		}
		if(argv[0].equalsIgnoreCase("window")){
			query1(argv[1], argv[2], argv[3], argv[4], argv[5]);
			}
		else if(argv[0].equalsIgnoreCase("within")){
			query2(argv[1], argv[2]);
		}
		else if(argv[0].equalsIgnoreCase("nearest-neighbor")){
			query3(argv[1], argv[2], argv[3]);
		}
		else if(argv[0].equalsIgnoreCase("fixed")){
			query4(argv[1]);
		}
	}
}
