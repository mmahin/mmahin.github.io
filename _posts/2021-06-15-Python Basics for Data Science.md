% Read a CSV file using the pandas library</br>
% Headers are the name you want for your variables within the dataframe</br>
headers = ["header 1", "header 2", ..., "header n"]</br>
% Data types are releated data types for the variables. Datatypes supported by python https://www.w3schools.com/python/python_datatypes.asp</br>
dtypes = {"header 1": 'type 1', "header 2": 'type 2',..., "header n": 'type 3'}</br>
% Path is the directory where the file is</br>
data = pd.read_csv(path, names=headers, dtype=dtypes)</br>
