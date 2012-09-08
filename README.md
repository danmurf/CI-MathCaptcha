# CodeIgniter Math Captcha v0.3 Alpha

A simple math based CAPTCHA library for CodeIgniter with various configurable options.

* Supports addition and multiplication
* Multi-language support
* Questions written in plain English (or your chosen language) in various phrases
* Questions can contain numbers as words, numeric or random
* Answers can be enforced to numeric, words or either
* Add your own question phrases for added randomness

##Requirements

This library is designed to work with CodeIgniter's core Session library. I've tested it with the core Form Validation library, but see no reason why you couldn't use it with an alternative.

Some examples:

> What do you get if you add eight to five?

> What is the sum of five and three?

> What is seven plus six?

or

> What is 7 plus 6?

##Installation

The folder structure should match the CodeIgniter folder structure.

###Quick setup

1. Copy the `application/libraries/mathcaptcha_library.php` file to `application/libraries/`
2. Copy the `application/language/english/mathcaptcha_lang.php` to `application/language/english/`
3. If you would like to use another language other than English you will need to duplicate the language file and translate the numbers and phrases respectively
4. Initialise the math CAPTCHA library and include it in your controller. Example below:

	    public function myform()
	    {
                $this->load->library('mathcaptcha');
                $this->mathcaptcha->init();
            
                $data['math_captcha_question'] = $this->mathcaptcha->get_question();
            
                $this->form_validation->set_rules('math_captcha', 'Math CAPTCHA', 'required|callback__check_math_captcha');
            
                if ($this->form_validation->run() == FALSE)
                {
                    $this->load->view('myform', $data);
                }
                else
                {
                    $this->load->view('myform', $data);
                }
	    }

5. Add a callback for the math CAPTCHA form validation if you are using CodeIgniter Form Validation library. Example below:
        
        function _check_math_captcha($str)
        {
            if ($this->mathcaptcha->check_answer($str))
            {
                return TRUE;
            }
            else
            {
                $this->form_validation->set_message('_check_math_captcha', 'Enter a valid math captcha response.');
                return FALSE;
            }
        }

6. Print the `$mathcaptcha_question` on your form somewhere. Example below:

        <?php echo validation_errors(); ?>
    
        <?php echo form_open(); ?>
    
            <?php echo $math_captcha_question;?>
    
            <?php echo form_input('math_captcha');?>
    
            <?php echo form_submit('submit', 'Submit'); ?>
    
        <?php echo form_close();?>

And that's it!

##Configuration options

There are some configuration options which you can pass to the library in an associative array when you `$this->mathcaptcha->init($config)`:

*   **language**: This should be set to the language that you want to use. It will default to the language set in the Codeigniter `config.php`.
*   **operation**: The type of math question to use; `addition` or `multiplication`. This will default to `addition` if not specified.
*   **question_format**: The type of number to include in the question; `numeric`, `word`, or `random`. This will default to `word` if not specified.
*   **question_max_number_size**: The maximum number size to use in the question. The default is `10`, which is also the maximum allowed given the limitations of the language file.
*   **answer_format**: The type of answer that is allowed; `word` means the user must answer in a word, `numeric` means the user must enter the number or `either` for, well, either.

In order to make your installation of math CAPTCHA more unique you can try changing/adding more phrases to the language file. If you add more than 5, adjust the `MATHCAPTCHA_NUM_ADDITION_PHRASES` and `MATHCAPTCHA_NUM_MULTIPLICATION_PHRASES` constants in the library file appropriately.

##License

GNU General Public License

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.
 
This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.