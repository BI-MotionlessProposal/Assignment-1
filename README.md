# Group: Motionless Proposal
Athinodoros Sgouromallis, Emmely Lundberg, Martin Macej


# List the all files that this program generates.
  -/synced_folder/assignments/assignment_1/price_list.csv
  -/synced_folder/assignments/assignment_1/price_list.txt
  -/synced_folder/assignments/assignment_1/prices.png
  
# Describe which types of files this program generates and attach the contents of each file together with its name to your solution.

# What is the output of this program?
  ## -The mean of the house prices.          
   ## -CSV file with a columns: 
                            -street                       
                            -city                     
                            -price                
                            -sqm                  
                            -price_per_sqm  
                            
## -PNG graph with graphical representation of the mean

# Describe in natural language and line-by-line what the program is doing. Describe also for each line what the Python code expresses.

          ## importing the os module (Miscellaneous operating system interfaces)
          import os
          ## importing the csv module (CSV File Reading and Writing)
          import csv
          ## importing the request module (Python HTTP for Humans)
          import requests
          ## importing the platform module (Access to underlying platform’s identifying data)
          import platform
          ## importing the statistics module (Mathematical statistics functions)
          import statistics
          ## importing the statistics module (A Python 2D plotting)
          import matplotlib
          ## setting the type of renderer so that it can output a png file
          matplotlib.use('agg')
          ## importing the matplotlib.pyplot module  (that Provides a MATLAB-like plotting framework)
          import matplotlib.pyplot as plt



          def run():
          # Assigns URL to be fetched to file_url
              file_url = 'https://raw.githubusercontent.com/datsoftlyngby/' \
                         'soft2017fall-business-intelligence-teaching-material/master/' \
                         'assignments/assignment_1/price_list.txt'
          # Return the base filename of pathname path.
              txt_file_name = os.path.basename(file_url)
          # Join one or more path components intelligently.
              txt_path = os.path.join('./', txt_file_name)
          # calling the download_txt function
              download_txt(file_url, txt_path)
          # creating a variable called csv_file_name  with the destination file name
              csv_file_name = 'price_list.csv'
          #  creating a variable called csv_path joining Path arguments with 
          #  os.getcwd() returning the current directory plus the file name 
              csv_path = os.path.join(os.getcwd(), csv_file_name)
          # calling the generate_csv function 
              generate_csv(txt_path, csv_path)
          # calling the read_prices function that returns list that data points to.
              data = read_prices(csv_path)
          # calling the compute_avg_price function and setting avg_price to point to the result
              avg_price = compute_avg_price(data)
          # print -python built in function: Prints out avg_price.
              print(avg_price)
          # calling the generate_plot function
              generate_plot(data)

          # defining download_txt() function with input parameters (url, path. Path has a default 
          #value so that if no argument is passed it takes the predefined one)
          def download_txt(url, save_path='./downloaded'):
          #Uses the function get of request module. Giver.
              response = requests.get(url)
          # Opens a stream with the python function open giving the path of the file, and modes (-b for binary and -w for write) as a parameter. 
          # The stream returns a File Object that a variable f points to.
              with open(save_path, 'wb') as f
          # Uses the File Object method write to write out the content of the response method.
                  f.write(response.content)

          # generate_csv - this function generates a csv file giving the two in parameters input path and the output csv file.
          def generate_csv(txt_input_path, csv_output_path):
           # it opens the file using ‘utf-8’ encoding
              with open(txt_input_path, encoding='utf-8') as f:
          # the list of the lines is returned and stored as txt_content
                  txt_content = f.readlines()
          # Instantiate a list giving its first element value a list of strings.
              rows = [['street', 'city', 'price', 'sqm', 'price_per_sqm']]
          # starting a loop block for each item in the txt_content list
              for line in txt_content:
          # replaces the * char in the beginning of the single line item with an empty string
                  line = line.rstrip().replace('  * ', '')
          # creating variables   address, price and sqm and assign values automatically 
          # from the list returned from the split function that splits the line on tab characters
          # each item will take the value of their index address for example will take line.split(“\t”)[0]
          # f.x. a,b,c = [1,2,3] will result a=1 , b=2 and c=3
                  address, price, sqm = line.split('\t')
           # creates  street and  city variables and assigns values values that result from from the split 
          # method as mentioned above.
                  street, city = address.split('; ')
          # creates price_per_sqm  and assigns the int result of the 
          # division of the parsed int values of price and sqm
                  price_per_sqm = int(price) // int(sqm)
          # Instantiate the variable row of the type tuple and values of the four variables.
                  row = (street, city, price, sqm, price_per_sqm)
          # Appending the tuple variable row to the list of rows.
                  rows.append(row)
          # Checking underlying system, if windows than newline variable is set to be ‘’ else None
              if platform.system() == 'Windows':
                  newline=''
              else:
                  newline=None
          #open the stream with the path of the files, mode -w (write), newline variable and encoding set #to ‘utf-8’ as input parameters. 
              with open(csv_output_path, 'w', newline=newline, encoding='utf-8') as f:
          # Uses the method write of the csv module giving the variable f that reference the File 
          # Object as input parameter. The method returns a writer object responsible for converting the user’s data into delimited strings on the given file-like object. The write object is assigned to the variable output_writer.
           output_writer = csv.writer(f)
          #iterate over the list rows and assigning it to the variable row
                  for row in rows:
          #Uses the method writerow of the writer object. The writerow method writes the row parameter to the writer’s file object, formatted according to the current dialect.
                      output_writer.writerow(row)

          # Defining function with path to csv file as an input parameter
          def read_prices(csv_input_path):
          # Opening csv file using utf-8 encoding and setting is as variable f
              with open(csv_input_path, encoding='utf-8') as f:
          # Returning list of lines from csv files
                  reader = csv.reader(f)
          # Flushing first line away (skips the headers)
                  _ = next(reader)

                  idxs = []
                  prices = []
          # Looping through each row in the list (reader)
                  for row in reader:
          # Underscores are in this case used to ignore undesired values
          # also assigns a value to price which is the third element in a row (row[2])
                      _, _, price, _, _ = row
          # Populates the idxs with the line number
                      idxs.append(reader.line_num)
          # Populates the prices with the captured price value
                      prices.append(int(price))
          #zip(*iterables )- Make an iterator that aggregates elements from each of the iterables.
          # creating a list of tuples where every tuple is made up by elements of idxs and prices. 
              return list(zip(idxs, prices))

          #defining compute_avg_price() function with input parameter data
          def compute_avg_price(data):
          # by calling the zip function with an asterisk zip will unpack the list given to sublists 
          # then disregards the indexes by assigning them to _ but keeps a reference to the prices 
              _, prices = zip(*data)
          #calls the mean function from the statistics library with the prices list as argument 
          # and assigns the mean result to avg_price  
              avg_price = statistics.mean(prices)
           # it opens the file giving the path in the first argument, the -w for write mode in the second argument and encoding ‘utf-8’. The stream returns a File Object that a variable f points to.
              with open('/tmp/avg_price.txt', 'w', encoding='utf-8') as f:
          # Uses the File Object method write to write out the avg_price after it has been converted to a string.
                  f.write(str(avg_price))
          #The function returns the variable avg_price
              return avg_price

          # Defining a function called generate_plot with one parameter
          def generate_plot(data):
          #Assigning x_values and y_values by unzipping data.
              x_values, y_values = zip(*data)
          #Instantiate a matplotlib.figure.Figure in fig
              fig = plt.figure()
          # Makes a scatter plot of (x_values, y_values) with the kwarg s=100.Marker size is scaled by s
          # Call savefig with the filename and the kwarg called bbox_inches set to tight
              fig.savefig('./prices.png', bbox_inches='tight')


          # If the script is ran directly (not imported), it will invoke run() otherwise it will not do anything
          if __name__ == '__main__':
              run()




