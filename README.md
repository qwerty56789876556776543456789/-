printf("Введи размер поля (x): ");
while (scanf("%d", &size) != 1 || size < 3) {
    while(getchar() != '\n');
    printf("Не то Введи целое число >= 3: ");
}




char **board = (char**)malloc(size * sizeof(char*));
for (int i = 0; i < size; i++) {
    board[i] = (char*)malloc(size * sizeof(char));
    for (int j = 0; j < size; j++) {
        board[i][j] = ' ';
    }
}





int mode;
printf("Выбери режим 1 - с человеком ( или не очень ), 2 - со мной : ");
while (scanf("%d", &mode) != 1 || (mode != 1 && mode != 2)) {
    while(getchar() != '\n');
    printf("... 1 или 2. ни чего другого от тебя не жду: ");
}

char user_sym = ' ', bot_sym = ' ';







if (mode == 2) {
    printf("Кто ты есть, воин?: ");
    while (scanf(" %c", &user_sym) != 1 || (toupper(user_sym) != 'X' && toupper(user_sym) != 'O')) {
        while(getchar() != '\n');
        printf("Ты дурак?): ");
    }
    user_sym = toupper(user_sym);
    bot_sym = (user_sym == 'X') ? 'O' : 'X';
    printf("ты  %c, я %c\n", user_sym, bot_sym);
}





int turn = 0; // 0 - ты (X), 1 - твой друг (O)
char current_sym = 'X';

while (1) {
    print_board(board, size);






    if (mode == 2 && current_sym == bot_sym) {
        printf("Подожди. я думаю\n");
        bot_move(board, size, bot_sym, user_sym);
    } else {
        char move_sym = (mode == 2) ? user_sym : current_sym;
        player_move(board, size, move_sym);
    }








    char check_sym = (mode == 2 && current_sym == bot_sym) ? bot_sym : ((mode == 2) ? user_sym : current_sym);
    if (check_win(board, size, check_sym)) {
        print_board(board, size);
        if (mode == 2) {
            if (check_sym == user_sym) printf("Поздравляю, ты сильнее меня)\n");
            else printf("я победил).\n");
        } else {
            printf("ты %c победил\n", check_sym);
        }
        break;
    }






    
    if (is_full(board, size)) {
        print_board(board, size);
        printf("Будь сильнее, а пока ты на ровне со мной \n");
        break;
    }





    turn = 1 - turn;
    current_sym = (turn == 0) ? 'X' : 'O';
}





for (int i = 0; i < size; i++) {
    free(board[i]);
}
free(board);

return 0;
