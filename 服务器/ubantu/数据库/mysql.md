### 连接mysql
  - npm install mysql2 koa
  ```js
    const Koa = require('koa');
    const mysql = require('mysql2/promise');

    const app = new Koa();

    // MySQL数据库配置
    const dbConfig = {
      host: 'localhost',
      user: 'your_username',
      password: 'your_password',
      database: 'your_database'
    };

    // 异步函数来获取数据库连接
    async function getDBConnection() {
      const connection = await mysql.createConnection(dbConfig);
      return connection;
    }

    // Koa中间件来处理请求
    app.use(async (ctx) => {
      try {
        const connection = await getDBConnection();
        
        // 示例查询
        const [rows] = await connection.execute('SELECT * FROM your_table');
        
        // 将查询结果发送给客户端
        ctx.body = rows;

        // 关闭数据库连接
        await connection.end();
      } catch (error) {
        ctx.status = 500;
        ctx.body = 'Internal Server Error';
        console.error('Database connection or query failed', error);
      }
    });

    // 启动服务器
    const PORT = 3000;
    app.listen(PORT, () => {
      console.log(`Server running on port ${PORT}`);
    });

  ```
  - npm install sequelize mysql2

  ```js 
    const { Sequelize } = require('sequelize');

    // 创建一个 Sequelize 实例
    const sequelize = new Sequelize('database', 'username', 'password', {
      host: 'localhost',
      dialect: 'mysql'
    });

    module.exports = sequelize;
    // 使用 Sequelize 定义模型，这些模型代表数据库中的表。例如，如果你有一个用户表，你可以创建一个 User 模型：
    const { DataTypes } = require('sequelize');
    const sequelize = require('./database'); // 引入上面创建的 Sequelize 实例

    const User = sequelize.define('User', {
      // 定义模型属性
      id: {
        type: DataTypes.INTEGER,
        autoIncrement: true,
        primaryKey: true
      },
      username: {
        type: DataTypes.STRING,
        allowNull: false
      },
      password: {
        type: DataTypes.STRING,
        allowNull: false
      }
      // 你可以添加更多的字段
    }, {
      // 其他模型选项
    });

    module.exports = User;
    // 在你的应用程序启动时，你可能想要同步你的模型到数据库。这可以通过调用 Sequelize 的 sync 方法来完成：
    const sequelize = require('./database'); // 引入 Sequelize 实例
    const User = require('./user'); // 引入模型

    async function synchronizeDatabases() {
      try {
        await sequelize.authenticate(); // 测试连接是否成功
        console.log('Connection has been established successfully.');

        // 同步所有模型
        await sequelize.sync({ force: true }); // 警告: 使用 `force: true` 将删除所有现有表
        console.log('All models were synchronized successfully.');
      } catch (error) {
        console.error('Unable to connect to the database:', error);
      }
    }

    synchronizeDatabases();
    > 注意： force: true 会删除所有现有的表并重新创建它们。在生产环境中，你应该非常小心地使用这个选项，或者使用迁移来管理数据库的变化。

    const User = require('./user'); // 引入 User 模型

    // 创建一个新用户
    async function createUser(username, password) {
      try {
        const newUser = await User.create({ username, password });
        console.log('User created:', newUser);
      } catch (error) {
        console.error('Error creating user:', error);
      }
    }

    // 调用函数创建用户
    createUser('john_doe', '123456');




