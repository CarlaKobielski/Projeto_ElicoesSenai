# Projeto_ElicoesSenai


package controller;

import java.util.List;

import dao.CandidatoDao;
import eleicoes.Candidato;

public class CandidatoController {
	
	public void salvar(Candidato candidato) throws Exception {
		if (Candidato.getNome() == null || ((Candidato) Candidato.getNome()).length() <=3) {
			throw new Exception("Nome Inválido");
		
		}
		CandidatoDao.getInstance().salvar(candidato);
	}
	
public void atualizar (Candidato candidato) throws Exception {
	if (Candidato.getNome() == null || ((Candidato) Candidato.getNome()).length()<=3) {
		throw new Exception("Nome Inválido");
	}
	CandidatoDao.getInstance().atualizar(candidato);
}

public void excluir (int idcandidato) throws Exception {
	if(idcandidato == 0) {
		throw new Exception("Nenhum Candidato Selecionado");
	}
	
	CandidatoDao.getInstance().excluir(idcandidato);
}
public List<Candidato> listar() {
		return CandidatoDao.getInstance().listar();
}
}
	
  
  
  
  Continuação 2° parte
  
  
  package controller;

import java.util.List;

import dao.CandidatoPDao;
import eleicoes.CandidatoPesquisa;

public class CanditatoPController {
	
	public static final CandidatoPesquisa CandidatoPesquisa = null;

	public class CandidatoController {
		
		public void salvar(CandidatoPesquisa candidato) throws Exception {
			if (eleicoes.CandidatoPesquisa.getDescricao() == null || ((CandidatoPesquisa) eleicoes.CandidatoPesquisa.getDescricao()).trim().equals("")) {
				throw new Exception("Descriçăo do produto inválida");
			}
			CandidatoPDao.getInstance().salvar(CandidatoPesquisa);
		}
		
	public void atualizar (CandidatoPesquisa candidato) throws Exception {
		if (eleicoes.CandidatoPesquisa.getDescricao() == null || ((CandidatoPesquisa) eleicoes.CandidatoPesquisa.getDescricao()).trim().equals("")) {
			throw new Exception("Descriçăo do produto inválida");
		}
		CandidatoPDao.getInstance().atualizar(CandidatoPesquisa);
	}

	public void excluir (int idcandidatoPesquisa) {
		CandidatoPDao.getInstance().excluir(CandidatoPesquisa);
		
	}
	public List<CandidatoPesquisa> listar(){
			return null;
	}
}
}



Continuação 3° parte

package controller;

import dao.CandidatoDao;
import eleicoes.Pesquisa;

public class PesquisaController {
	
	
	

	public void registrarPesquisa(Pesquisa pesquisa) throws Exception {
		if (Pesquisa.getdataPesquisa == null) {
			throw new Exception("Data Inválida");
		}
	
		if (Pesquisa.getCandidato() == null) {
			throw new Exception("Candidato Inválido");
		}
		CandidatoDao.getInstance().registrarPesquisa(pesquisa);
	
	}
}
	
	
PACOTE DAO 1° PARTE

package dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;

import eleicoes.Candidato;
import eleicoes.Pesquisa;

public class CandidatoDao {
	
	private static CandidatoDao instance;
	private List <Candidato> listaCandidato = new ArrayList <>();
	private Statement ConnectionUtil;
	private Connection con = ConnectionUtil.getConnection();
		public static CandidatoDao getInstance() {
		if (instance == null) {
			instance= new CandidatoDao();
			
		}
			return instance;
	}
	

public void salvar(Candidato candidato) {
	listaCandidato.add(candidato);
	
	try {
		String sql = "insert into Candidato (nome, cpf, endereco, telefone) values (?, ?, ?, ?)";
		PreparedStatement pstmt = con.prepareStatement(sql);
		pstmt.setString(1,Candidato.getNome());
		pstmt.setString(2, Candidato.getIdCandidato());
		pstmt.setString(3, Candidato.getLocalPesquisa());
		pstmt.setString(4,Candidato.getFichaLimpaCandidato());
		pstmt.execute();
	} catch (SQLException e) {
		e.printStackTrace();
	}

		
	}
	
public void atualizar (Candidato candidato) throws SQLException {
	listaCandidato.set(candidato.getId(), candidato);
	try {
	String sql = "update Candidato set nome = ?, cpf = ?, endereco = ?, telefone = ? where id = ?";
	PreparedStatement pstmt = con.prepareStatement(sql);
	pstmt.setString(1,Candidato.getNome());
	pstmt.setString(2, Candidato.getIdCandidato());
	pstmt.setString(3, Candidato.getLocalPesquisa());
	pstmt.setString(4,Candidato.getFichaLimpaCandidato());
	pstmt.execute();
}   
	catch (SQLException e) {
	e.printStackTrace();
}
}

public void excluir (int idcandidato) {
	listaCandidato.remove(idcandidato);
	
	try {
		String sql = "delete from Candidato where id = ?";
		PreparedStatement pstmt = con.prepareStatement(sql);
		pstmt.setInt(1, idcandidato);
		pstmt.executeUpdate();
	} catch (SQLException e) {
		e.printStackTrace();
	}
}

public List<Candidato> listar(){
		return listaCandidato;
		try {
			String sql = "select * from Candidato";
			Statement stmt = con.createStatement();
			ResultSet rs = stmt.executeQuery(sql);
			while (rs.next()) {
				Candidato c = new Candidato();
				c.setId(rs.getInt("idcandidato"));
				c.setNome(rs.getString("Nome"));
				c.setLocaPesquisa(rs.getString("Cidade: "));
				c.setfichaLimpaCandidato(rs.getString("Candidato é Ficha Limpa? "));
				listaCandidato.add(c);
			}
			catch (SQLException e) {
				e.printStackTrace();
			}
			return listaCandidato;
		

}
		
		



public void registrarPesquisa(Pesquisa pesquisa) {
}
}



PACOTE DAO 2° PARTE

package dao;

import java.beans.Statement;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;
import util.ConnectionUtil;

import eleicoes.Candidato;
import eleicoes.CandidatoPesquisa;

public class CandidatoPDao {

	
private Connection con;

public void salvar(CandidatoPesquisa candidato) {
	List<CandidatoPesquisa> listaCandidatoPesquisa = new ArrayList<>();
	private Connection con = ConnectionUtil.getConnection();
	
	CandidatoDao getInstance; {
		if (getInstance == null) {
			getInstance = new CandidatoDao();
		}
		return;
	}
}
public void atualizar (CandidatoPesquisa candidato) {
	
	try {
		String sql = "update into Candidato (Nome, id, ficha Limpa (?, ?)";
		PreparedStatement pstmt = con.prepareStatement(sql);
		pstmt.setString(1, CandidatoPesquisa.getNome());
		pstmt.setString(2, CandidatoPesquisa.getIdCandidatoPesquisa());
		pstmt.setString(3,Candidato.getFichaLimpaCandidato());
		pstmt.execute();
	} catch (SQLException e) {
		e.printStackTrace();
	}
	
}


public void excluir (int idcandidatoPesquisa, int idCandidato) {
	try {
		String sql = "delete from Candidato where id = ?";
		PreparedStatement pstmt = con.prepareStatement(sql);
		pstmt.setInt(1, idCandidato);
		pstmt.execute();
	} catch (SQLException e) {
		e.printStackTrace();
	}
}

public List<CandidatoPesquisa> listar(List<CandidatoPesquisa> listaCandidato){
	try {
		String sql = "select * from produto";
		java.sql.Statement stmt = con.createStatement();
		ResultSet rs = ((java.sql.Statement) stmt).executeQuery(sql);
		while (rs.next()) {
			Candidato p = new Candidato();
			p.setId(rs.getInt("id"));
			p.setString(1, CandidatoPesquisa.getNome());
			p.setString(3,Candidato.getFichaLimpaCandidato());
			listaCandidato.add(p);
		}
	} catch (SQLException e) {
		e.printStackTrace();
	}
	return listaCandidato;
}


public static CandidatoDao getInstance() {
	// TODO Auto-generated method stub
	return null;
}
	
	public static Object getDescricao() {
		// TODO Auto-generated method stub
		return null;
}

	public void excluir(CandidatoPesquisa candidatopesquisa) {
		// TODO Auto-generated method stub
		
	}
}


PACOTE DAO 3° PARTE

package dao;

import java.beans.Statement;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;
import util.ConnectionUtil;
import eleicoes.Candidato;
import eleicoes.Pesquisa;

public class PesquisaDao {
	
	private static PesquisaDao instance;
	private List<Pesquisa> listaPesquisa = new ArrayList<>();
	private Connection con = ConnectionUtil.getConnection();
	
	public static PesquisaDao getInstance() {
		if (instance == null) {
			instance = new PesquisaDao();
		}
		return instance;
	}
	public void registrarPesquisa(Pesquisa venda) {
		try {
			
			String sql = "insert into  dados pesquisa) values (?, ?, ?)"; 
			
			PreparedStatement pstmt = con.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);
			
			pstmt.setString(1, java.sql.Date.valueOf(Pesquisa.rs("id")));
			PreparedStatement pstmt1 = con.prepareStatement(sql);
			pstmt1.setString(1,Candidato.getNome());
			pstmt1.setString(2, Candidato.getIdCandidato());
			pstmt1.setString(3, Candidato.getLocalPesquisa());
			pstmt1.setString(4,Candidato.getFichaLimpaCandidato());
			
			int key = pstmt1.executeUpdate();
			
			if (key > 0) {
			
				ResultSet rs = pstmt1.getGeneratedKeys();
				rs.next();
				int idVenda = rs.getInt(1);
			
			
				String sqlItemVenda = "insert into dados pesquisa) values (?, ?, ?, ?)";
				PreparedStatement pstmtItemVenda = con.prepareStatement(sqlItemVenda);
				for (Pesquisa item : Pesquisa.getPesquisa()) {
					pstmt1.setString(1,Candidato.getNome());
					pstmt1.setString(2, Candidato.getIdCandidato());
					pstmt1.setString(3, Candidato.getLocalPesquisa());
					pstmt1.setString(4,Candidato.getFichaLimpaCandidato());
					
				}
			}
			
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
}











