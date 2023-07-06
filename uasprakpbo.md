Source Code UAS :

// CLASS PERTAMA

package uasprakpbo;

import java.io.BufferedReader;    

import java.io.IOException;   

import java.io.InputStreamReader; 

import java.sql.*; 

/**
 *
 * @author Bintangrizqia
 */

public class dbkoneksi {
    private String url;     
    private String username; 
    private String password; 
    
    static InputStreamReader InputStreamReader = new InputStreamReader(System.in);

    static BufferedReader input = new BufferedReader(InputStreamReader);

    public dbkoneksi(String url, String username, String password) {
        this.url = url;          
        this.username = username; 
        this.password = password; 
    
}
    
    public void showMenu (){  // menampilkan seluruh case pada menu
    System.out.println ();
    System.out.println (" --------------------------- ");
    System.out.println ("       YOUR TODO LIST ");
    System.out.println (" --------------------------- ");
    System.out.println (" 1. Create Data ");
    System.out.println (" 2. Show Data ");
    System.out.println (" 3. Update Data ");
    System.out.println (" 4. Delete Data ");
    System.out.println (" 0. Back ");
    System.out.println ("");
    System.out.print   (" PILIHAN > ");
}
    
        public void displayData() {  // method utk menampilkan data yang terdapat dalam database
        try {
            Class.forName("com.mysql.jdbc.Driver");                                       
            Connection connection = DriverManager.getConnection(url, username, password); 
            Statement statement = connection.createStatement();  
            String query = "SELECT * FROM todolist";        
            ResultSet resultSet = statement.executeQuery(query); 
            
            System.out.println ();                                
            System.out.println (" ===== TODO LIST  ===== "); 
            int i = 1;  
            
            while (resultSet.next()) {
                int id = resultSet.getInt("id");          
                String todo = resultSet.getString("todo"); 
                String kategori = resultSet.getString("kategori");      
                String tgl = resultSet.getString("tgl_selesai"); 
                String status = resultSet.getString("status");      
                System.out.println();
                
            
                System.out.println(String.format(" id.%d \n Todo \t\t : %s \n Kategori \t : %s \n Tanggal Selesai : %s \n Status \t : %s", i++, todo, kategori, tgl, status ));
                System.out.println();
            }
            
            resultSet.close();
            statement.close();  
            connection.close();
        } catch (ClassNotFoundException e) {                 
            System.out.println("Failed to load JDBC driver"); 
            e.printStackTrace();  
        } catch (SQLException e) {                         
            System.out.println("Database connection error"); 
            e.printStackTrace(); 
        } 
    }
        
        public void insertData(){ 
        try {
            Class.forName("com.mysql.jdbc.Driver");                                    
            Connection connection = DriverManager.getConnection(url, username, password); 
            Statement statement = connection.createStatement(); 
            String query = "INSERT INTO todolist";        

            int i = 1;                
                System.out.println ();
                System.out.print(" Todo            : ");
                String todo = input.readLine().trim();         
                System.out.print(" Kategori        : ");
                String kategori = input.readLine().trim();  
                System.out.print(" Tanggal Selesai : ");
                String tgl = input.readLine().trim();
                System.out.print(" Status          : ");
                String status = input.readLine().trim();         
                System.out.println ();
                
        
                String sql = " INSERT INTO todolist (todo, kategori, tgl_selesai, status) VALUES ('"+todo+"', '"+kategori+"', '"+tgl+"', '"+status+"')";
                sql = String.format(sql, todo, kategori, tgl, status);
                
             
                statement.execute(sql);
                
            } catch (Exception e) {  
              e.printStackTrace(); 
            
        }  
    }
        
         public void updateData(){  
        try {
            Class.forName("com.mysql.jdbc.Driver");                                      
            Connection connection = DriverManager.getConnection(url, username, password);
            Statement statement = connection.createStatement(); 
            
            System.out.println ();                
            System.out.print (" Todo Sebelumnya     : "); 
            String todo = input.readLine().trim(); 
            String query = "SELECT ID FROM todolist where todo = '"+todo+"'";  
            ResultSet resultSet = statement.executeQuery(query);  
            
            int id = 0;  // dekl sekaligus inisialisasi id bertipe int dgn nil awal 0
            while (resultSet.next()) {
            id = resultSet.getInt ("ID"); // mengambil nilai kolom "ID" dari setiap baris data
            } 
            
            System.out.print(" Todo Baru          : ");
            todo = input.readLine().trim();
            System.out.print(" kategori           : ");
            String kategori = input.readLine().trim();
            System.out.print(" Tanggal Selesai    : ");
            String tgl = input.readLine().trim();
            System.out.print(" Status             : ");
            String status = input.readLine().trim();

            query = " UPDATE todolist SET todo='"+todo+"', kategori='"+kategori+"', tgl_selesai='"+tgl+"', status='"+status+"' WHERE id='"+id+"'";
            query = String.format (query, id, todo, kategori, tgl, status);
            
           
            statement.execute(query);
            
            } catch (Exception e) {
              e.printStackTrace();
        }  
    }
         
          public void deleteData(){ 
        try {
            Class.forName("com.mysql.jdbc.Driver");                                     
            Connection connection = DriverManager.getConnection(url, username, password); 
            Statement statement = connection.createStatement();  
            
            System.out.print (" Masukkan TODO yang akan dihapus : ");
            String todo = input.readLine(); // baca inputan user
            String query = ("DELETE FROM todolist WHERE todo = '"+todo+"'");
            System.out.println (" Data telah terhapus ...");
   
            statement.execute(query);
  
            }catch (Exception e) {
             e.printStackTrace();
             }    
        }  
}


// CLASS KEDUA
package uasprakpbo;
import static uasprakpbo.dbkoneksi.input;

public class UASPRAKPBO {
    public static void main(String[] args) {
         String url = "jdbc:mysql://localhost/todo_list";
        String username = "root";
        String password = "";
        
        // digunakan utk melakukan operasi ke database
        dbkoneksi reader = new dbkoneksi(url, username, password);
        reader.showMenu(); // memanggil dan menjalankan method 
        
        try {
            while (true){
        int pilihan = Integer.parseInt(input.readLine()); // membaca inputan user
        
        switch (pilihan) {
            case 0:
                 System.exit(0);
            break;
            case 1:
                 reader.insertData();  // memanggil method insertData()
            break;
            case 2:
                 reader.displayData(); // memanggil method displayData()
            break;
            case 3:
                 reader.updateData();  // memanggil method updateData()
            break;
            case 4:
                 reader.deleteData();  // memanggil method deleteData()
            break;
            default:
                  // cetak pesan jika nomor yg diinput tdk ada pd case
                  System.out.println (" Pilihan Salah!"); 
        }
        // digunakan looping while agar dapat memilih ulang case
        System.out.print (" PILIHAN > "); 
            }  
        } catch (Exception e) {
          e.printStackTrace();
        }
    }
}



