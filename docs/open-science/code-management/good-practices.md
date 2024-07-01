
Writing code is an essential aspect of research and data management, hence investing your time to acquire good coding habits is a fundamental skill to reduce mistakes and to make your research more transparent and reproducible.

!!! note "Invest on learning good coding"
    You are writing code for two final users: the **future you** and **other researchers**. Make sure that both these final users are able to understand and re-use your code. Write your code with them in mind and they will be surely greatful.


As a starting point, you should make sure to gain a few principles of good coding. These principles are simple and especially easy to implement, we strongly suggest you to adopt them immediately.

## Good coding practices:

- **Use meaningful variables:** Assign variable names that reflect the role of a given variable. 

      *Bad*
      ```python
      a = 12
      b = 3.14
      ```

      *Good*
      ```python
      radius = 12
      pi = 3.14
      ```
      
- **Avoid hard coding (at any cost)**: Hard coding is the dangerous habit of using throughout your code a plain value instead of assigning a variable name as an alias.</b>  Hard coding makes your code extremely unclear (e.g. it is difficult to identify what a numerical value means) and very unflexible (e.g. changing hard coded values throughout your code increases the chance of bugs that you might not even be aware of).

      *Bad*
      ```python
      a = 3.14 * 5**2
      ```

      *Good*
      ```python
      pi = 3.14
      radius = 5
      circle_area = pi * radius**2
      ```

- **Keep it simple:** if you have the choice between a clever, but convoluted solution, and less clever, but simpler solution, **choose the simplest solution**. It is probably the easiest version to read and understand in the future.

- **Comment your code:** Although code can be understood only by reading code, other users and the future you will benefit from helpful comments to understand the logic you used and to get the gist of what you have done. The following is an example of commented python code. Each programming language has its own peculiar way of writing comments, please refer to the section about language specific styles.  

    ```python
    def square_me(num): 
        """
        Parameters
        ----------
        num : int/float
            Number to get the power of
        
        Returns
        -------
        int/float
            Power of the input number
        """
        return num**2

    ```

- **Chunk your code:** Chunk your code in small modular parts when possible (pretty much always). By using **functions** or **methods** of a class you will be able to divide your long and complicated code in small parts that are easier to handle and to test. If you have the chance restrict a function or a method to just one task, this will drastically reduce the error space.

- **Do not repeat yourself:** If you need to use multiple times throughout your code a piece of code that achieves a specific goal, create a function out of it and call it when necessary. It makes your code more readable and reduces the chance of creating bugs.

- **Make it readable:** Code can easily become convoluted and hard to read, try to make it readable by following style and guidelines of your language of choice and adopting the previous points of this list.

- **Read other people code:** Reading other people code teaches you how more advance coders approach a problem and write their own code. At the same time you are also going to encounter a huge amount of bad code, this will help you understand what not to do (see also antipatterns in [python](https://docs.quantifiedcode.com/python-anti-patterns/) and [R](https://www.burns-stat.com/pages/Tutor/R_inferno.pdf) to know what bad habits to avoid). 

- **Use helpful tools**: Use [linters](https://en.wikipedia.org/wiki/Lint_(software)) and other softwares that can help you identify issues in your code, from bugs to from style. 
    - **Matlab:** [codeIssues](https://www.mathworks.com/help/matlab/ref/codeissues.html)
    - **Python:** [Black](https://black.readthedocs.io/en/stable/), [Pylint](https://pylint.readthedocs.io/en/latest/) and [Flake8](https://flake8.pycqa.org/en/latest/)
    - **R:** [Lintr](https://lintr.r-lib.org/)

- **Test your code:** Take the time to build tests for your code. Sistematically testing your code, although tedious and time consuming, might save your project from nasty mistakes:
    - [Matlab](https://www.mathworks.com/help/matlab/matlab-unit-test-framework.html)
    - [R](https://r-pkgs.org/testing-basics.html)
    - [Python](https://docs.python-guide.org/writing/tests/)
  
- **Be patient and put time and effort on it:** Coding does not always come easy, it needs patience, time, effort and practice.

## Language specific styles and principles:

Although the previous principles apply to most of the programming languages you will encounter, each language follows its own peculiar conventions.
Style conventions are not the result of a pure aestethic choice, but they aim at specific practical goals, such as increasing readibility and reducing the chance to produce bugs. 

**Styles and conventions of the major languages used in cognitive neuroscience:**
As a rule of thumb, you do not need to memorize all the language conventions, but it would be a good starting point to read the main guidelines and to start including them immediately in your code. With time you will be able to include more and more conventions into your code, but keep in mind that following some of them will be, at times, tedious and it might not come naturally, but stick to them and it will pay off.

- **Python:** [https://peps.python.org/pep-0008/#introduction](https://peps.python.org/pep-0008/#introduction)
- **Matlab:** [https://www.mathworks.com/matlabcentral/fileexchange/45047-matlab-style-guidelines-cheat-sheet](https://www.mathworks.com/matlabcentral/fileexchange/45047-matlab-style-guidelines-cheat-sheet)
- **R:** [https://google.github.io/styleguide/Rguide.html](https://google.github.io/styleguide/Rguide.html)
- **List of style guides for different programming languages:** [https://google.github.io/styleguide/](https://google.github.io/styleguide/)

## Version control: git

When coding your experiment or your analysis you will inevitably make multiple changes to your code and create multiple versions of the same code, therefore running the risk to pollute your folders and to produce avoidable bugs.
In order to prevent such potential issues, we recommend to use a version control software.</b> 

A version control software keeps track of your code changes for you and allows you to revert your code to previous versions at any time without creating extra files. Furthermore, it allows you to modify your code while keeping intact a working version and then merging the modified version with the old one.
The most common version control software is [**git**](https://git-scm.com/), a lightweight and very powerful tool. We are not going to cover it here, but we suggest you to go through this [tutorial](https://book.the-turing-way.org/reproducible-research/vcs/vcs-git-general) or any other online tutorial.

