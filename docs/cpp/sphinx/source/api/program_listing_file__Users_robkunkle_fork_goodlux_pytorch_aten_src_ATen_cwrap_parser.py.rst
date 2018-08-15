
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cwrap_parser.py:

Program Listing for File cwrap_parser.py
========================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cwrap_parser.py`

.. code-block:: cpp

   import yaml
   
   # follows similar logic to cwrap, ignores !inc, and just looks for [[]]
   
   
   def parse(filename):
       with open(filename, 'r') as file:
           declaration_lines = []
           declarations = []
           in_declaration = False
           for line in file.readlines():
               line = line.rstrip()
               if line == '[[':
                   declaration_lines = []
                   in_declaration = True
               elif line == ']]':
                   in_declaration = False
                   declaration = yaml.load('\n'.join(declaration_lines))
                   declarations.append(declaration)
               elif in_declaration:
                   declaration_lines.append(line)
           return declarations
