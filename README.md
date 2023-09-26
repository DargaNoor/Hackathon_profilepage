# Hackathon_profilepage
This is a basic Profile web page created using HTML,CSS for Hackathon.
import java.io.BufferedInputStream;
import java.io.ByteArrayOutputStream;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class DownloadBlob {

	private static void downloadBlob() {
		Connection conn = null; // Initialize connection / get connection
		PreparedStatement pst = null;
		try {
			String query = "select <blob_column> from <table> where <where_column>=?";
			String fileName = "download.xml";

			pst = conn.prepareStatement(query);
			pst.setInt(1, 100);

			ResultSet rs = pst.executeQuery();
			while (rs.next()) {
				InputStream is = rs.getBinaryStream(1);
				String str = convert(is);

				File file = new File(fileName);
				FileOutputStream outputStream = new FileOutputStream(file);
				outputStream.write(str.getBytes());
				outputStream.close();

				System.out.println("File Generated: " + fileName);
			}

		} catch (Exception e) {
			e.printStackTrace();
		}

		finally {
			try {
				pst.close();
				conn.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
	}

	private static String convert(InputStream is) {
		BufferedInputStream bis = new BufferedInputStream(is);
		ByteArrayOutputStream buf = new ByteArrayOutputStream();
		int result;
		String str = null;
		try {
			result = bis.read();

			while (result != -1) {
				buf.write((byte) result);
				result = bis.read();
			}
			str = buf.toString("UTF-8");
		} catch (IOException e) {
			e.printStackTrace();
		}
		return str;
	}

}
