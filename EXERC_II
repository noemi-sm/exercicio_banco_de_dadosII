CREATE TABLE clientes (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100),
    pontos INT DEFAULT 0
);

CREATE TABLE transacoes (
    id SERIAL PRIMARY KEY,
    cliente_id INT,
    valor INT,
    tipo VARCHAR(10),
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);

-- Criar a função que será chamada pelo trigger
CREATE OR REPLACE FUNCTION atualizar_pontos()
RETURNS TRIGGER AS $$
BEGIN
    IF NEW.tipo = 'Compra' THEN
        UPDATE clientes
        SET pontos = pontos + NEW.valor
        WHERE id = NEW.cliente_id;
    ELSIF NEW.tipo = 'Devolucao' THEN
        UPDATE clientes
        SET pontos = pontos - NEW.valor
        WHERE id = NEW.cliente_id;
    END IF;

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Criar o trigger
CREATE TRIGGER trigger_atualizar_pontos
AFTER INSERT ON transacoes
FOR EACH ROW
EXECUTE FUNCTION atualizar_pontos();
