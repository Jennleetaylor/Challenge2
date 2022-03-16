# Project Title

Enable users to save the qualifying loans as a new CSV file so that users can share the results more effeciently as a spreadsheet. Doing this simplifies the process and applications needed for the end user and allows for an easy to understand output of the qualifying loan results.

---

## Technologies

Describe the technologies required to use your project such as programming languages, libraries, frameworks, and operating systems. Be sure to include the specific versions of any critical dependencies that you have used in the stable version of your project.

1. Python
2. GitHub
3. Terminal
4. Fire
5. Questionary

---

## Installation Guide

In this section, you should include detailed installation notes containing code blocks and screenshots.
`pip install questionary`

1. Install Libraries into Panda. PY file.

  import sys
  import fire
  import questionary
  from pathlib import Path

2. From File Script - Save file type as a CSV:
  
  from qualifier.utils.fileio import load_csv, save_csv
  
3. Install scripts used for qualifying loans

    from qualifier.utils.calculators import (
    calculate_monthly_debt_ratio,
    calculate_loan_to_value_ratio,---

    
## Usage

1. From CSV File, we are filtering out the data to display in the CSV file:

  from qualifier.filters.max_loan_size import filter_max_loan_size
  from qualifier.filters.credit_score import filter_credit_score
  from qualifier.filters.debt_to_income import filter_debt_to_income
  from qualifier.filters.loan_to_value import filter_loan_to_value
  
2. Asking for the file path and CSV file to be created with the latest banking data.

  def load_bank_data():
    """Ask for the file path to the latest banking data and load the CSV file.

    Returns:
        The bank data from the data rate sheet CSV file.
    """

    csvpath = questionary.text("Enter a file path to a rate-sheet (.csv):").ask()
    csvpath = Path(csvpath)
    if not csvpath.exists():
        sys.exit(f"Oops! Can't find this path: {csvpath}")

    return load_csv(csvpath)
    
    
  3. Use the applicant info function to prompt a dialog box to the user to get the applicant's financial information:
    
    def get_applicant_info():
    """Prompt dialog to get the applicant's financial information.

    Returns:
        Returns the applicant's financial information.
    """

    credit_score = questionary.text("What's your credit score?").ask()
    debt = questionary.text("What's your current amount of monthly debt?").ask()
    income = questionary.text("What's your total monthly income?").ask()
    loan_amount = questionary.text("What's your desired loan amount?").ask()
    home_value = questionary.text("What's your home value?").ask()

    credit_score = int(credit_score)
    debt = float(debt)
    income = float(income)
    loan_amount = float(loan_amount)
    home_value = float(home_value)

    return credit_score, debt, income, loan_amount, home_value
    
 4. Use the qualifiying loans function to determine which loans the user qualifies for based on the loan qualificaion criteria:
def find_qualifying_loans(bank_data, credit_score, debt, income, loan, home_value):
    """Determine which loans the user qualifies for.

    Loan qualification criteria is based on:
        - Credit Score
        - Loan Size
        - Debit to Income ratio (calculated)
        - Loan to Value ratio (calculated)

    Args:
        bank_data (list): A list of bank data.
        credit_score (int): The applicant's current credit score.
        debt (float): The applicant's total monthly debt payments.
        income (float): The applicant's total monthly income.
        loan (float): The total loan amount applied for.
        home_value (float): The estimated home value.

    Returns:
        A list of the banks willing to underwrite the loan.

5. Wsing the function to calculate the monthly debt ratio and LTV ratio using values **Args**.

    # Calculate the monthly debt ratio
    monthly_debt_ratio = calculate_monthly_debt_ratio(debt, income)
    print(f"The monthly debt to income ratio is {monthly_debt_ratio:.02f}")

    # Calculate loan to value ratio
    loan_to_value_ratio = calculate_loan_to_value_ratio(loan, home_value)
    print(f"The loan to value ratio is {loan_to_value_ratio:.02f}.")

6. Running the qualification filters and returning the bank data filtered:   
   
   # Run qualification filters
    bank_data_filtered = filter_max_loan_size(loan, bank_data)
    bank_data_filtered = filter_credit_score(credit_score, bank_data_filtered)
    bank_data_filtered = filter_debt_to_income(monthly_debt_ratio, bank_data_filtered)
    bank_data_filtered = filter_loan_to_value(loan_to_value_ratio, bank_data_filtered)

    print(f"Found {len(bank_data_filtered)} qualifying loans")

    return bank_data_filtered

7. Saves the qualifying loans to a CSV file 

    def save_qualifying_loans(qualifying_loans):
    """Saves the qualifying loans to a CSV file.

    Args:
        qualifying_loans (list of lists): The qualifying bank loans.
    """
    # @TODO: Complete the usability dialog for savings the CSV Files.
    if not qualifying_loans:
        sys.exit(f"No qualifying loans were found.")
        
    savepath = questionary.text("Enter a file path to save the qualifying loans:").ask()
    savepath = Path(savepath)
    
    if savepath:
        save_csv(savepath, qualifying_loans)
        
8. Running the  main function for running the script:

  ef run():
    """The main function for running the script."""

    # Load the latest Bank data
    bank_data = load_bank_data()

    # Get the applicant's information
    credit_score, debt, income, loan_amount, home_value = get_applicant_info()

    # Find qualifying loans
    qualifying_loans = find_qualifying_loans(
        bank_data, credit_score, debt, income, loan_amount, home_value
    )

    # Save qualifying loans
    save_qualifying_loans(qualifying_loans)

    if __name__ == "__main__":
    fire.Fire(run)


## Contributors

Jennifer Taylor (jennleetaylor21@gmail.com


## License

When you share a project on a repository, especially a public one, it's important to choose the right license to specify what others can and can't with your source code and files. Use this section to include the license you want to use.
