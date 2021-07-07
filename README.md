Program.cs

using System;
using MySql.Data.MySqlClient;

namespace Version
{
    class Program
    {
        static void Main(string[] args)
        {
            string cs = @"server=localhost;userid=dbuser;password=s$cret;database=testdb";

            using var con = new MySqlConnection(cs);
            con.Open();

            Console.WriteLine($"MySQL version : {con.ServerVersion}");
        }
    }
}

MySQL with a SELECT statement.

Program.cs

using System;
using MySql.Data.MySqlClient;

namespace SelectStatement
{
    class Program
    {
        static void Main(string[] args)
        {
            string cs = @"server=localhost;userid=dbuser;password=s$cret;database=mydb";

            using var con = new MySqlConnection(cs);
            con.Open();

            var stm = "SELECT VERSION()";
            var cmd = new MySqlCommand(stm, con);

            var version = cmd.ExecuteScalar().ToString();
            Console.WriteLine($"MySQL version: {version}");
        }
    }
}

 create a database table and fill it with data.

Program.cs
using System;
using MySql.Data.MySqlClient;

namespace CreateTable
{
    class Program
    {
        static void Main(string[] args)
        {
            string cs = @"server=localhost;userid=dbuser;password=s$cret;database=testdb";

            using var con = new MySqlConnection(cs);
            con.Open();

            using var cmd = new MySqlCommand();
            cmd.Connection = con;

            cmd.CommandText = "DROP TABLE IF EXISTS cars";
            cmd.ExecuteNonQuery();

            cmd.CommandText = @"CREATE TABLE cars(id INTEGER PRIMARY KEY AUTO_INCREMENT,
                    name TEXT, price INT)";
            cmd.ExecuteNonQuery();

            cmd.CommandText = "INSERT INTO cars(name, price) VALUES('Audi',52642)";
            cmd.ExecuteNonQuery();

            cmd.CommandText = "INSERT INTO cars(name, price) VALUES('Mercedes',57127)";
            cmd.ExecuteNonQuery();

            cmd.CommandText = "INSERT INTO cars(name, price) VALUES('Skoda',9000)";
            cmd.ExecuteNonQuery();

            cmd.CommandText = "INSERT INTO cars(name, price) VALUES('Volvo',29000)";
            cmd.ExecuteNonQuery();

            cmd.CommandText = "INSERT INTO cars(name, price) VALUES('Bentley',350000)";
            cmd.ExecuteNonQuery();

            cmd.CommandText = "INSERT INTO cars(name, price) VALUES('Citroen',21000)";
            cmd.ExecuteNonQuery();

            cmd.CommandText = "INSERT INTO cars(name, price) VALUES('Hummer',41400)";
            cmd.ExecuteNonQuery();

            cmd.CommandText = "INSERT INTO cars(name, price) VALUES('Volkswagen',21600)";
            cmd.ExecuteNonQuery();

            Console.WriteLine("Table cars created");
        }
    }
}

Prepared statements increase security and performance. When we write prepared statements, we use placeholders instead of directly writing the values into the statements.

Program.cs
using System;
using MySql.Data.MySqlClient;

namespace PreparedStatement
{
    class Program
    {
        static void Main(string[] args)
        {
            string cs = @"server=localhost;userid=dbuser;password=s$cret;database=testdb";

            using var con = new MySqlConnection(cs);
            con.Open();

            var sql = "INSERT INTO cars(name, price) VALUES(@name, @price)";
            using var cmd = new MySqlCommand(sql, con);

            cmd.Parameters.AddWithValue("@name", "BMW");
            cmd.Parameters.AddWithValue("@price", 36600);
            cmd.Prepare();

            cmd.ExecuteNonQuery();

            Console.WriteLine("row inserted");
        }
    }
}

C# MySqlDataReader

using System;
using MySql.Data.MySqlClient;

namespace RetrieveCars
{
    class Program
    {
        static void Main(string[] args)
        {
            string cs = @"server=localhost;userid=dbuser;password=s$cret;database=testdb";

            using var con = new MySqlConnection(cs);
            con.Open();

            string sql = "SELECT * FROM cars";
            using var cmd = new MySqlCommand(sql, con);

            using MySqlDataReader rdr = cmd.ExecuteReader();

            while (rdr.Read())
            {
                Console.WriteLine("{0} {1} {2}", rdr.GetInt32(0), rdr.GetString(1), 
                        rdr.GetInt32(2));
            }
        }
    }
}

C# MySQL column headers

using System;
using MySql.Data.MySqlClient;

namespace ColumnHeaders
{
    class Program
    {
        static void Main(string[] args)
        {
            string cs = @"server=localhost;userid=dbuser;password=s$cret;database=testdb";

            using var con = new MySqlConnection(cs);
            con.Open();

            var sql = "SELECT * FROM cars";

            using var cmd = new MySqlCommand(sql, con);

            using MySqlDataReader rdr = cmd.ExecuteReader();
            Console.WriteLine($"{rdr.GetName(0),-4} {rdr.GetName(1),-10} {rdr.GetName(2),10}");

            while (rdr.Read())
            {
                Console.WriteLine($"{rdr.GetInt32(0),-4} {rdr.GetString(1),-10} {rdr.GetInt32(2),10}");
            }
        }
    }
}

