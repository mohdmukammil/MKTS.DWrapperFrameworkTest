# MKTS.DWrapperFrameworkTest

using DWrapperFramework;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace TestDWrapperFramework
{
    public class Program
    {
        static void Main(string[] args)
        {
            string connectionString = "Data Source=.\\SqlExpress;Initial Catalog=TestMKWrapper;Integrated Security=True;Pooling=False";
            Program p = new Program();
            //Test1
            //p.Insert(connectionString);

            //Test2
            //p.InsertConnectionOriented(connectionString);

            //Test3
            //p.InsertList(connectionString);

            //Test4
            //p.InsertListConnectionOriented(connectionString);

            //Test5
            //p.SelectSingle(connectionString);

            //Test6
            //p.SelectAll(connectionString);

            //Test7
            //p.Select(connectionString);

            //Test8
            //p.Update(connectionString);

            //Test9
            //p.UpdateConnectionOriented(connectionString);

            //Test10
            //p.Delete(connectionString);

            //Test11
            //p.DeleteConnectionOriented(connectionString);

            //Test12
            //p.DWrapperMapSP(connectionString);

            //Test13
            p.GetItems(connectionString);

            //Test14
            //p.Execute(connectionString);
        }

        #region Insert Test
        public void Insert(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);

            Employee emp = new Employee();
            emp.Name = "Test";
            emp.City = "Roorkee";
            emp.Salary = 10000;
            wrapper.Insert<Employee>(emp);

            Console.WriteLine("Id " + emp.Id);
            Console.WriteLine("Name " + emp.Name);
            Console.WriteLine("City " + emp.City);
        }

        public void InsertConnectionOriented(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);
            using (wrapper.GetOpenConnection())
            {
                using (var transaction=wrapper.BeginTransaction())
                {
                    try
                    {
                        Employee emp = new Employee();
                        emp.Name = "Test";
                        emp.City = "Roorkee";
                        emp.Salary = 10000;
                        wrapper.InsertConnectionOriented<Employee>(emp);

                        Console.WriteLine("Id " + emp.Id);
                        Console.WriteLine("Name " + emp.Name);
                        Console.WriteLine("City " + emp.City);
                        transaction.Commit();
                    }
                    catch(Exception ex)
                    {
                        transaction.Rollback();
                    }
                }
            }
        }

        public void InsertList(string connectionString)
        {

            DWrapper wrapper = new DWrapper(connectionString);
            List<Employee> empList = new List<Employee>();
            Employee emp = new Employee();
            emp.Name = "ListTest1";
            emp.City = "Roorkee";
            emp.Salary = 10000;
            empList.Add(emp);

            Employee empNew = new Employee();
            empNew.Name = "ListTest2";
            empNew.City = "Roorkee";
            empNew.Salary = 10000;
            empList.Add(empNew);

            wrapper.InsertList<Employee>(empList);


        }

        public void InsertListConnectionOriented(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);
            using (wrapper.GetOpenConnection())
            {
                using (var transaction = wrapper.BeginTransaction())
                {
                    try
                    {
                        List<Employee> empList = new List<Employee>();
                        Employee emp = new Employee();
                        emp.Name = "ListTest1";
                        emp.City = "Roorkee";
                        emp.Salary = 10000;
                        empList.Add(emp);

                        Employee empNew = new Employee();
                        empNew.Name = "ListTest2";
                        empNew.City = "Roorkee";
                        empNew.Salary = 10000;
                        empList.Add(empNew);

                        wrapper.InsertListConnectionOriented<Employee>(empList);
                        transaction.Commit();
                    }
                    catch (Exception ex)
                    {
                        transaction.Rollback();
                    }
                }
            }
        }
        #endregion

        #region Select Test
        public void SelectSingle(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);
            Employee emp = wrapper.SelectSingle<Employee>(new { Id=1});
            Console.WriteLine("Id " + emp.Id);
            Console.WriteLine("Name " + emp.Name);
            Console.WriteLine("City " + emp.City);
        }

        public void SelectAll(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);
            List<Employee> empList = wrapper.SelectAll<Employee>()?.ToList();

            foreach (var emp in empList)
            {
                Console.WriteLine("Id " + emp.Id);
                Console.WriteLine("Name " + emp.Name);
                Console.WriteLine("City " + emp.City);
            }
        }

        public void Select(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);
            Employee emptemp = new Employee();
            emptemp.Id = 1;
            List<Employee> empList = wrapper.Select<Employee>(new { Name="Test" })?.ToList();
            foreach (var emp in empList)
            {
                Console.WriteLine("Id " + emp.Id);
                Console.WriteLine("Name " + emp.Name);
                Console.WriteLine("City " + emp.City);
            }
        }
        #endregion

        #region Update Test
        public void Update(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);

            Employee emp = new Employee();
            emp.Name = "UpdateTest";
            emp.City = "Roorkee";
            emp.Salary = 10000;
            emp.Id = 1;
            wrapper.Update<Employee>(emp);

            Console.WriteLine("Id " + emp.Id);
            Console.WriteLine("Name " + emp.Name);
            Console.WriteLine("City " + emp.City);
        }

        public void UpdateSelectedColumns(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);

            Employee emp = new Employee();
            emp.Name = "Test Selected";
            emp.City = "Roorkee";
            emp.Salary = 10000;
            // Id is mandatory param
            wrapper.UpdateSelectedColumns<Employee>(new { Id = 1, Name = "Update Selected" });

            Console.WriteLine("Id " + emp.Id);
            Console.WriteLine("Name " + emp.Name);
            Console.WriteLine("City " + emp.City);

        }
            

        public void UpdateConnectionOriented(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);
            using (wrapper.GetOpenConnection())
            {
                using (var transaction = wrapper.BeginTransaction())
                {
                    try
                    {
                        Employee emp = new Employee();
                        emp.Name = "UpdateConnectionOriented";
                        emp.City = "Roorkee";
                        emp.Salary = 10000;
                        emp.Id = 1;
                        wrapper.UpdateConnectionOriented<Employee>(emp);

                        Console.WriteLine("Id " + emp.Id);
                        Console.WriteLine("Name " + emp.Name);
                        Console.WriteLine("City " + emp.City);
                        transaction.Commit();
                    }
                    catch (Exception ex)
                    {
                        transaction.Rollback();
                    }
                }
            }
        }
        #endregion

        #region Delete Test
        public void Delete(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);
            Employee emp = new Employee();
            emp.Name = "Test";
            emp.City = "Roorkee";
            emp.Salary = 10000;
            emp.Id = 1;
            wrapper.Delete<Employee>(emp);

            emp.City = "Delhi";

            Console.WriteLine("Id " + emp.Id);
            Console.WriteLine("Name " + emp.Name);
            Console.WriteLine("City " + emp.City);
        }

        public void DeleteConnectionOriented(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);
            using (wrapper.GetOpenConnection())
            {
                using (var transaction = wrapper.BeginTransaction())
                {
                    try
                    {
                        Employee emp = new Employee();
                        emp.Name = "Test";
                        emp.City = "Roorkee";
                        emp.Salary = 10000;
                        emp.Id = 2;
                        wrapper.DeleteConnectionOriented<Employee>(emp);
                        transaction.Commit();
                    }
                    catch (Exception ex)
                    {
                        transaction.Rollback();
                    }
                }
            }
        }
        #endregion

        #region Stored Procedure Test
        public void DWrapperMapSP(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);
            var dynamicData=wrapper.DWrapperMapSP<dynamic>("sp_ReadEmployeeById", new { Id = 3 });
            //string objectString = JsonConvert.SerializeObject(dynamicData);
            if(dynamicData.Count>0)
            {
                foreach(var emp in dynamicData)
                {
                    Console.WriteLine("Name " + emp.Name);
                    Console.WriteLine("Salary " + emp.Salary);
                }
            }
            else
            {
                Console.WriteLine("Name " + dynamicData.Name);
                Console.WriteLine("Salary " + dynamicData.Salary);
            }
        }

        #endregion

        #region Direct Query Test
        public void GetItems(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);
            using (wrapper.GetOpenConnection())
            {
                var empList = wrapper.GetItems<dynamic>(System.Data.CommandType.Text, "Select * from Employee where id=@Id", new { Id = 4 });

            }
        }

        public void Execute(string connectionString)
        {
            DWrapper wrapper = new DWrapper(connectionString);
            using (wrapper.GetOpenConnection())
            {
                var empList = wrapper.Execute<dynamic>(System.Data.CommandType.Text, "Update Employee set name=@Name where id=@Id", new { Id = 8, Name = "Mukammil" });
            }
        }
        #endregion 
    }

    public class Employee
    {
        public int Id { get; set; }

        public string Name { get; set; }

        public string City { get; set; }

        public decimal Salary { get; set; }
    }
}
