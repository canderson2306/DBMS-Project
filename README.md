import java.io.IOException;
import java.sql.*;

public class SqlProject {

	public static void main(String[] args) throws SQLException, IOException, 
	ClassNotFoundException  {
		
System.out.println("Beginning");
		
		
		Connection conn = DriverManager.getConnection("jdbc:postgresql://localhost:5432/postgres", "postgres", "postgres");

		// For atomicity
		conn.setAutoCommit(false);

		// For isolation
		conn.setTransactionIsolation(Connection.TRANSACTION_SERIALIZABLE);

		Statement stmt1 = null;
		try {
			// Create statement object
			stmt1 = conn.createStatement();
			
			
			// Either the 2 following inserts are executed, or none of them are. This is
			// atomicity
			stmt1.executeUpdate("INSERT INTO Product (prodid, pname, price) VALUES ('p4', 'Printer', 100.00)");
			stmt1.executeUpdate("INSERT INTO Stock (prodid, depid, quantity) VALUES ('p4', 'd1', 15)");
		} catch (SQLException e) {
			System.out.println("An exception was thrown");
			e.printStackTrace();
			// For atomicity
			conn.rollback();
			stmt1.close();
			conn.close();
			return;
		}
		conn.commit();
		stmt1.close();
		conn.close();
		System.out.println("End");

	}

}
