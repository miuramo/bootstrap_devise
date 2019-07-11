# README

## Usage

```
$  git clone git@github.com:miuramo/bootstrap_devise.git
$  cd bootstrap_devise/
$  bundle install
$  rails db:migrate
$  rails s -b 0.0.0.0 -p 3428
```
* ポート番号(3428)を変えるときは、$ ./findgrep 3428 で、どのファイルを修正すべきか、確認して、変更してください。

---

# Ruby on Railsでdeviseを導入する(Bootstrap4+日本語化対応)

* https://qiita.com/MURAMASA2470/items/705b54d569e14979c413

# deviseとcancancanで会員登録と権限管理を行い、管理者だけにrails_adminを公開する

* https://qiita.com/iamdaisuke/items/79d60b3c23e465ae6460

 vi Gemfile
```
  gem 'cancancan'
  gem 'rails_admin'
```
 bundle install

 rails g cancan:ability

 rails g rails_admin:install

 vi config/initializers/rails_admin.rb 
```
  ## == Devise ==
  config.authenticate_with do
    warden.authenticate! scope: :user
  end
  config.current_user_method(&:current_user)

  ## == Cancan ==
  config.authorize_with :cancan
```

 rails g migration AddAdminFlgToUser admin_flg:boolean

 rails db:migrate

 rails c
```
  > user = User.find(1)
  > user.update_attribute(:admin_flg, true)
```
 vi app/models/ability.rb
```
  class Ability
    include CanCan::Ability

    def initialize(user)
      if user && user.admin_flg?
        can :access, :rails_admin
        can :manage, :all
      end
    end
  end
```
