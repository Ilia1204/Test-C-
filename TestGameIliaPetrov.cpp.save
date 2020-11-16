#include "TXLib.h"
#include <iostream>
#include <fstream>   //Подключение библиотеки,которая читает файлы
using namespace std; //Название имени пространства

struct Question
{
    string text_question;
    int count_answers;
    int fontSize;
    HDC fon;
    HDC Changretta;
    int right_answer;
    string text_answer[7];
};
void drawAnswer(int x, int y, string text)
{
    txRectangle(x, y,  x + 200, y + 80);
    txDrawText (x, y,  x + 200, y + 80, text.c_str());
}
bool focusAnswer(int x, int y)
{
   return (txMouseX() >= x && txMouseX() <= x + 200 &&
           txMouseY() >= y && txMouseY() <= y + 80);
}
string ReadParametr(string str, int *pos2)
{
    int pos1 = *pos2 + 1;
    *pos2 = str.find(";", pos1 + 1);
    string address = str.substr(pos1, *pos2 - pos1);
    return address;
}
int main()
{
    txCreateWindow (800, 600);
    txBegin();
    //Курсора нет
    txTextCursor(false);
    //Два раза нажимать не надо по завершению
    txDisableAutoPause();
    //номер Вопроса(переменная)
    int n_question = 1;
    //Кол-во правильных ответов
    int points = 0;
    //Массив вопросов
    Question question_list[15];
    //Промежуточная строка
    string str;
    //Открыли файл
    ifstream file("Вопросы.txt");
    //Номер элемента массива
    int n = 0;

    //цикл файла
    while(file.good())
    {
        //Читаем файл(строку)
        getline(file, str); //Текст,фон
        if(str.size() < 20) continue;

        //задаём вопрос
        int pos2 = -1;
        string text_question = ReadParametr(str,&pos2);
        question_list[n].text_question = text_question.c_str();

        char text_q[200] = "";      //Новая строка
        bool needToEnter = false;   //Пора \n
        int  count_Enters = 0;       //Сколько энтеров уже было


        //У тебя 100 букв. Через каждые 30 ставишь \n
        for (int i = 0; i < text_question.size(); i++)
        {
            //Сохраняем текущий символ
            char s = text_question[i];
            text_q[i + count_Enters] = s;

            //Дошли до 30, 60, 90 символа - пора Enter
            if (i % 30 == 0 && i > 0)
                needToEnter = true;

            //Если пробел
            if (needToEnter && text_question[i] == ' ')
            {
                needToEnter = false;        //Пока энтеров хватит

                //Добавляем энтер
                text_q[i + count_Enters] = '\n';

                //И уже прочитанный символ тоже добавляем в строку
                count_Enters = count_Enters + 1;
                char s1 = text_question[i];
                text_q[i + count_Enters] = s1;
            }
        }

        question_list[n].text_question = text_q;


        //задаём кол-во ответов
        string count_answers = ReadParametr(str,&pos2);
        question_list[n].count_answers = atoi(count_answers.c_str());

        //задаём размер шрифта
        string fontsize = ReadParametr(str,&pos2);
        question_list[n].fontSize = atoi(fontsize.c_str());

        //задаём фон
        string fon = ReadParametr(str,&pos2);
        question_list[n].fon = txLoadImage(fon.c_str());

        //Если фон не найден
        if(!question_list[n].fon)
           question_list[n].fon = question_list[0].fon;

        //картинка Чангретта в 2 вопросе
        if (str.size() > pos2)
        {
            string Changretta = ReadParametr(str,&pos2);
            question_list[n].Changretta = txLoadImage(Changretta.c_str());
        }


        //Ответы
        getline(file, str);
        if(str.size() < 10) continue;
        pos2 = -1;

        //задаём правильный ответ
        string right_answer = ReadParametr(str,&pos2);
        question_list[n].right_answer = atoi (right_answer.c_str());

        //цикл кол-во ответов
        for (int i = 0; i < question_list[n].count_answers; i++)
                            question_list[n].text_answer[i] = ReadParametr(str, &pos2);
        //Пустая
        getline(file, str);

       n++;

    }

    file.close(); //Закрыли файл

    //начинаем с первого(нулевого) вопроса
    Question question = question_list[0];

    //фоны и картинки
    HDC GreyFon = txLoadImage ("PicturesTest/GreyFon.bmp");
    HDC SmallHeath = txLoadImage ("PicturesTest/SmallHeath.bmp");
    HDC Birmingham = txLoadImage ("PicturesTest/Birmingham.bmp");
    HDC fon = GreyFon;
    HDC Changretta = txLoadImage ("PicturesTest/Changretta.bmp");
    HDC CamdenTown = txLoadImage ("PicturesTest/CamdenTown.bmp");

   while(n_question <= n)
   {
        txBegin();
        question = question_list[n_question - 1];

        txSetFillColor(TX_BLACK);
        txClear();

        txBitBlt (txDC(), 0, 0, txGetExtentX(), 600,question.fon);

        if(question.Changretta != nullptr)
           txBitBlt (txDC(), 270, 120, txGetExtentX(), 200, question.Changretta,0,0);



        //Вопрос
        txSetColor(TX_BLACK,5);
        txSelectFont("Arial", 55);
        txDrawText(0,0, txGetExtentX(), txGetExtentY() / 2, question.text_question.c_str());

        //Выводит сколько всего вопросов и на каком сейчас находишься
        txSetColor(TX_BLACK,5);
        txSelectFont("Arial", 30,15);
        char n_questionStr[100];
        sprintf(n_questionStr,"Вопрос %d/%d",n_question,13);
        txTextOut(0 ,260,n_questionStr);


        //Ответы
        txSetFillColor(TX_BLACK);
        txSetColor(TX_WHITE,5);

        //Сначала рисуются все ответы
        txSelectFont("Arial",30);
        drawAnswer(40, 420, question.text_answer[0]);
        drawAnswer(295, 370, question.text_answer[1]);
        drawAnswer(545, 420, question.text_answer[2]);
        if(question.count_answers >= 4)
           drawAnswer(285, 495, question.text_answer[3]);

        //кнопки на которые надо нажимать
        //1 ответ
        if(focusAnswer(40, 420))
        {
          txSelectFont("Arial",33);
          drawAnswer(40, 420, question.text_answer[0]);
        }
        if(txMouseButtons() == 1 && focusAnswer(40, 420))
          {
            if(question.right_answer == 1)
            {
              txSetColor(TX_GREEN,5);
              //Звуковой эффект немного запаздывает(жди немного после клика)
              txPlaySound("RightAnswer.wav",SND_ASYNC);
              points++;
            }
            else
            {
              //Звуковой эффект немного запаздывает(жди немного после клика)
              txPlaySound("IncorrectAnswer.wav",SND_ASYNC);
              txSetColor(TX_RED,5);
            }

            n_question++;
            drawAnswer(40, 420, question.text_answer[0]);
            txSleep(500);
          }


        //2 ответ
        if(focusAnswer(295, 370))
        {
          txSelectFont("Arial",33);
          drawAnswer(295, 370, question.text_answer[1]);
        }
        if(txMouseButtons() == 1 && focusAnswer(295, 370))
          {
            if(question.right_answer == 2)
            {
              txSetColor(TX_GREEN,5);
              //Звуковой эффект немного запаздывает(жди немного после клика)
              txPlaySound("RightAnswer.wav",SND_ASYNC);
              points++;
            }
            else
            {
              //Звуковой эффект немного запаздывает(жди немного после клика)
              txPlaySound("IncorrectAnswer.wav",SND_ASYNC);
              txSetColor(TX_RED,5);
            }

            n_question++;
            drawAnswer(295, 370, question.text_answer[1]);
            txSleep(500);
          }


        //3 ответ
        if(focusAnswer(545, 420))
        {
          txSelectFont("Arial",33);
          drawAnswer(545, 420, question.text_answer[2]);
        }
        if(txMouseButtons() == 1 && focusAnswer(545, 420))
          {
            if(question.right_answer == 3)
            {
              txSetColor(TX_GREEN,5);
              //Звуковой эффект немного запаздывает(жди немного после клика)
              txPlaySound("RightAnswer.wav",SND_ASYNC);
              points++;
            }
            else
            {
              //Звуковой эффект немного запаздывает(жди немного после клика)
              txPlaySound("IncorrectAnswer.wav",SND_ASYNC);
              txSetColor(TX_RED,5);
            }

            n_question++;
            drawAnswer(545, 420, question.text_answer[2]);
            txSleep(500);
        }


        //4 ответ
        if (focusAnswer(285, 495))
        {
             txSelectFont("Arial",33);
             drawAnswer(285, 495, question.text_answer[3]);
          }
        if(txMouseButtons() == 1 && focusAnswer(285, 490))
           {
            if(question.right_answer == 4)
            {
              txSetColor(TX_GREEN,5);
              //Звуковой эффект немного запаздывает(жди немного после клика)
              txPlaySound("RightAnswer.wav",SND_ASYNC);
              points++;
            }
            else
            {
              //Звуковой эффект немного запаздывает(жди немного после клика)
              txPlaySound("IncorrectAnswer.wav",SND_ASYNC);
              txSetColor(TX_RED,5);
            }

            n_question++;
            drawAnswer(285, 495, question.text_answer[3]);
            txSleep(500);
           }


      txEnd();
      txSleep(100);
    }

     //есть возможность получить разную оценку
     txSetFillColor(TX_BLACK);
     txClear();
     txSelectFont("Arial",50);
     txSetColor(TX_WHITE,5);
     if(points >= 13)
      txTextOut(100, 100,"Да, ты чёртов острый козырёк!");
     else if (points >= 10)
      txTextOut(100, 100,"Немного подготовься и вперёд!");
     else if (points >= 5)
      txTextOut(100, 100,"Тебе стоит пересмотреть сериал.");
     else if (points >= 3)
      txTextOut(100, 100,"Ты вообще смотрел сериал???");
     else if (points >= 0)
      txTextOut(100, 100,"Ты меня разочаровал...");


    //в конце итог(сколько очков)
    char pointsStr[100];
    sprintf(pointsStr,"Очков %d/%d",points,13);
    txTextOut(500,0,pointsStr);


    //пауза
    txSleep(3000);

    //удаляем картинки
    txDeleteDC (Changretta);
    for (int i = 0; i < n; i++)
         txDeleteDC(question_list[i].fon);

    return 0;
}

