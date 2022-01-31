# Bot development plan

---

> scope

- Scope
  - static : global_variables `[{variable_name : str, variable_value : object}]`
  - static : stages `[Stage]`
  - static : users `[User]`
  - static : main_stage `Stage`
  - `change_variable(variable_name, variable_value)`
      - `global_variables[variable_name] = variable_value`
  
  - `get_variable(variable_name)`
      - `global_variables[variable_name]`



> stage

- Stage
  - static : main_stage `Stage`
  - name `str`
  - message `Message | {function : func, results : {function_result :  Message}}`
  - next_stage_name `str? | {function : func, results : {function_result :  str}}?`
  - user_input_action `[ActionBase]? | {function : func, results : {function_result :  [Action]}}?`
  - user_input_filter `InputFilter? | {function : func, results : {function_result :  InputFilter}}?`
  
> message

- Message
  - text `MessageText? | {function : func, results : {function_result : MessageText}}?`
  - picture `MessagePicture? | {function : func, results : {function_result : MessagePicture}}?`
  - keyboard `MessageKeyboard? | {function : func, results : {function_result : MessageKeyboard}}?`


- MessageText
  - text `str`
  - parse_mode `parseMode`


- MessagePicture
  - picture_file_disk_source `str`
  - picture_file_telegram_id `str`


- MessageKeyboard
  - buttons `[MessageKeyboardButton]`
  - is_non_keyboard_input_allowed `bool`


- MessageKeyboardButton
  - text `str`
  - actions `[Action]`

> action

- Action
  - action_function `function?`


- ActionBack < Action
  - `action_function = user, scope => user.stage_history <<`


- ActionBackToMain < Action
  - `action_function = user, scope => user.change_stage(scope.main_stage)`


- ActionChangeStage < Action
  - stage_name `str`
  - `action_function = user, scope => user.change_stage(stage_name)`


- ActionChangeUserVariable < Action  
  - `action_function = user, scope => user.change_variable`

- ActionChangeGlobalVariable < Action
  - `action_function = user, scope => scope.change_variable`

> filter

- InputFilter
  - filter_regex `regex?`
  - filter_function `function?`
  - `approve(input_text)`
    - `check if input is correct`


- IntNumberFilter < InputFilter
  - `filter_regex = '[0-9]+'`


- DoubleNumberFilter < InputFilter
  - `filter_regex = '[0-9]+[,.][0-9]+'`

> condition

- Condition
  - condition_function `function?`

> user

- User
  - stage_history `[str]`
  - user_variables `[{variable_name : str, variable_value : object}]`
  - chat_id `str`

  - `__init__(chat_id)`
    - `return user from db or create new with stage 'NewUser'`

  - `change_stage(stage_name)`
      - `user.stage_history + stage_name`
      - `send new stage message`

  - `change_variable(variable_name, variable_value)`
      - `user_variables[variable_name] = variable_value`
  
  - `get_variable(variable_name)`
      - `user_variables[variable_name]`

> bot

- Bot
  - `process_message(user_id):`

