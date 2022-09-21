# ProgramaJava
Problemas de ID com o BD
Aqui está minha classe usuarioDAO
nao sei se no insert ou no select por id esta o erro, mas na depuração o codigo crasha quando faço o select, ele insere o usuario e senha mas me retorna um Id=0. como resolver?


     public class UsuarioDAO {
    private final Connection connection;

    public UsuarioDAO(Connection connection) {
        this.connection = connection;
    }

    
    public Usuario insert(Usuario usuario) throws SQLException{
               // conexoes banco dados
 
            String sql = "insert into usuario(usuario,senha) values (?,?)";
            
            PreparedStatement statement = connection.prepareStatement(sql);
            statement.setString(1,usuario.getUsuario());
            statement.setString(2,usuario.getSenha());
            statement.execute(); 
            
            ResultSet resultSet = statement.getGeneratedKeys();            
            if (resultSet.next()){
                int id = resultSet.getInt("id");
                usuario.setId(id);
                
            }
            return usuario;
                        
        
           }
           public void update(Usuario usuario) throws SQLException{
            String sql = "update usuario set usuario = ?, senha = ? where id = ? ";
            PreparedStatement statement = connection.prepareStatement(sql);
            
            statement.setString(1,usuario.getUsuario());
            statement.setString(2,usuario.getSenha());
            statement.setInt(3,usuario.getId());
            statement.execute();
            }
    
           public void insertOrUpdate(Usuario usuario) throws SQLException{
            if(usuario.getId() > 0){
            update(usuario);
             }else{
            insert(usuario);
            }
            }
    
           public void delete(Usuario usuario) throws SQLException{
           String sql = "delete from usuario where id = ?";
           PreparedStatement statement = connection.prepareStatement(sql);        
            statement.setInt(1,usuario.getId());
            statement.execute();
        
            }
          public ArrayList<Usuario> selectAll() throws SQLException{
          String sql = "select * from usuario";
          PreparedStatement statement = connection.prepareStatement(sql);
        
          ArrayList<Usuario> usuarios = new ArrayList<>();
         
          statement.execute();
          ResultSet resultSet = statement.getResultSet();
        
           while(resultSet.next()){
            int id = resultSet.getInt("id");
            String usuario = resultSet.getString("usuario");
            String senha =   resultSet.getString("senha");
            
            Usuario usuarioComDados = new Usuario(id, usuario, senha);
            usuarios.add(usuarioComDados);
            }
           return usuarios;
           }  
 
           private ArrayList<Usuario> pesquisa(PreparedStatement statement) throws SQLException {
           ArrayList<Usuario> usuarios = new ArrayList<>();
        
           statement.execute();
           ResultSet resultSet = statement.getResultSet();
        
           while(resultSet.next()){
            int id = resultSet.getInt("id");
            String usuario = resultSet.getString("usuario");
            String senha = resultSet.getString("senha"); 
            
            Usuario usuarioComDados = new Usuario(id, usuario, senha);
            usuarios.add(usuarioComDados);
             }
        
             return usuarios;
           }  
    
            /**
            *
              * @param usuario
            * @return
          * @throws SQLException
           */
           public Usuario selectPorId(Usuario usuario) throws SQLException{
           String sql = "select * from usuario where id = ? ";
          PreparedStatement statement = connection.prepareStatement(sql);        
          statement.setInt(1, usuario.getId());
        
          ArrayList<Usuario> usuarios = pesquisa(statement);
        
           return usuarios.get(0);
               
        
          }

          public boolean existeNoBancoPorUsuarioESenha(Usuario usuario) throws SQLException {
           String sql = "select * from usuario where usuario = ? and senha = ?";
        
           PreparedStatement statement = connection.prepareStatement(sql);
           statement.setString(1, usuario.getUsuario());
           statement.setString(2, usuario.getSenha());
           statement.execute();
        
        
           ResultSet resultSet = statement.getResultSet();       
          return resultSet.next();
           }

    
          }



