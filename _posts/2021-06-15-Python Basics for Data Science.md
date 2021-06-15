# Read a CSV file using the pandas library
# Headers are the name you want for your variables within the dataframe
headers = ["header 1", "header 2", ..., "header n"]
# Data types are releated data types for the variables. Datatypes supported by python https://www.w3schools.com/python/python_datatypes.asp
dtypes = {"header 1": 'type 1', "header 2": 'type 2',..., "header n": 'type 3'}
# Path is the directory where the file is
data = pd.read_csv(path, names=headers, dtype=dtypes)
